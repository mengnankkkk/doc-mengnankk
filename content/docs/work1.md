---
title: 开发项目总结
categories:
  - 项目
  
abbrlink: 2701
date: 2025.4.26
tags: 
   - java
   - spring boot
   - redis
---



# 总体架构

## 架构思想：

**分层**：

Controller → Service → Entity，这三层架构体系，

`Controller` 层 **不直接操作数据库**，而是通过 `subjectService` 去拿数据。

业务逻辑集中在 `Service` 层，`Controller` 只负责**接收请求、调用服务、返回结果**。

**统一的返回结果**：

不直接返回裸的 `List` 或 `对象`，而是包一层 `RestResponse`。

成功返回 `RestResponse.ok(数据)`。

统一格式，前端处理简单。

后期可以很方便统一加异常码、消息、分页信息。

**合理的使用对象映射：**

`Subject` 是实体类（Entity），对应数据库。

`SubjectVM`、`SubjectEditRequestVM` 是视图模型（VM），对应前端页面。

通过 `modelMapper.map(d, SubjectVM.class)` 进行转换，不暴露数据库结构。进行反序列化，更加安全

**使用流式编程：**

```
List<SubjectVM> subjectVMS = subjects.stream().map(d -> {
    SubjectVM subjectVM = modelMapper.map(d, SubjectVM.class);
    subjectVM.setId(String.valueOf(d.getId()));
    return subjectVM;
}).collect(Collectors.toList());

```

使用 `stream().map(...).collect(...)`，
 一次性把 `List<Subject>` 转换成 `List<SubjectVM>`，代码简洁、可读性高。



# common方法模板

## 根据id查询

```java
@PostMapping("/read/{id}")
public RestResponse<ExamPaperReadVM> read(@PathVariable Integer id) {
    // 查询实体
    ExamPaperAnswer answer = examPaperAnswerService.selectById(id);
    // 转 VM
    ExamPaperReadVM vm = new ExamPaperReadVM();
    vm.setPaper(examPaperService.examPaperToVM(answer.getExamPaperId()));
    vm.setAnswer(examPaperAnswerService.examPaperAnswerToVM(answer.getId()));
    // 返回结果
    return RestResponse.ok(vm);
}

```

查询实体->转成视图vm->结果

## 新增

```java
@PostMapping("/add")
public RestResponse<Integer> add(@RequestBody SubjectEditRequestVM model) {
    // VM → Entity
    Subject entity = modelMapper.map(model, Subject.class);
    subjectService.insert(entity);
    // 返回新 ID
    return RestResponse.ok(entity.getId());
}

```

vm->转为实体->插入返回id

```java
@PostMapping("/answerSubmit")
public RestResponse answerSubmit(@RequestBody @Valid ExamPaperSubmitVM vm) {
    User user = getCurrentUser();
    ExamPaperAnswerInfo info = examPaperAnswerService.calculateExamPaperAnswer(vm, user);
    if (info == null) {
        return RestResponse.fail(2, "试卷不能重复做");
    }

    // 计算结果 & 事件
    ExamPaperAnswer answer = info.getExamPaperAnswer();
    String scoreVm = ExamUtil.scoreToVM(answer.getUserScore());
    eventPublisher.publishEvent(new CalculateExamPaperAnswerCompleteEvent(info));
    eventPublisher.publishEvent(new UserEvent(new UserEventLog(
        user.getId(), user.getUserName(), user.getRealName(), new Date(),
        user.getUserName() + " 提交试卷：" + info.getExamPaper().getName() +
        " 得分：" + scoreVm + " 耗时：" + ExamUtil.secondToVM(answer.getDoTime())
    )));

    return RestResponse.ok(scoreVm);
}

```



## 修改

```java
@PostMapping("/edit")
public RestResponse<Void> edit(@RequestBody SubjectEditRequestVM model) {
    // VM → Entity
    Subject entity = modelMapper.map(model, Subject.class);
    subjectService.updateById(entity);
    // 返回成功
    return RestResponse.ok();
}

```

跟新增逻辑差不多

```java
@PostMapping("/edit")
public RestResponse edit(@RequestBody @Valid ExamPaperSubmitVM vm) {
    // 校验 & 状态检查
    if (vm.getAnswerItems().stream().anyMatch(i -> i.getDoRight()==null && i.getScore()==null)) {
        return RestResponse.fail(2, "有未批改题目");
    }
    ExamPaperAnswer answer = examPaperAnswerService.selectById(vm.getId());
    if (ExamPaperAnswerStatusEnum.fromCode(answer.getStatus()) == ExamPaperAnswerStatusEnum.Complete) {
        return RestResponse.fail(3, "试卷已完成");
    }

    // 业务 & 事件
    String score = examPaperAnswerService.judge(vm);
    User user = getCurrentUser();
    eventPublisher.publishEvent(new UserEvent(new UserEventLog(
        user.getId(), user.getUserName(), user.getRealName(), new Date(),
        user.getUserName() + "批改试卷" + answer.getPaperName() + "得分" + score
    )));

    return RestResponse.ok(score);
}

```



## 删除

```java
@PostMapping("/delete/{id}")
public RestResponse<Void> delete(@PathVariable Integer id) {
    subjectService.deleteById(id);
    return RestResponse.ok();
}

```

## 分页查询

```java
@PostMapping("/pageList")
public RestResponse<PageInfo<ExamPaperAnswerPageResponseVM>> pagelist(
    @RequestBody @Valid ExamPaperAnswerPageVM model) {

    // ——— 固定模板 ———
    model.setCreateUser(getCurrentUser().getId());
    PageInfo<ExamPaperAnswer> pageInfo = examPaperAnswerService.studentPage(model);

    // ——— 流式转换 & 工具调用 ———
    PageInfo<ExamPaperAnswerPageResponseVM> page = PageInfoHelper.copyMap(pageInfo, e -> {
        ExamPaperAnswerPageResponseVM vm = modelMapper.map(e, ExamPaperAnswerPageResponseVM.class);
        vm.setSubjectName(subjectService.selectById(vm.getSubjectId()).getName());
        vm.setDoTime(ExamUtil.secondToVM(e.getDoTime()));
        vm.setSystemScore(ExamUtil.scoreToVM(e.getSystemScore()));
        vm.setUserScore(ExamUtil.scoreToVM(e.getUserScore()));
        vm.setPaperScore(ExamUtil.scoreToVM(e.getPaperScore()));
        vm.setCreateTime(DateTimeUtil.dateFormat(e.getCreateTime()));
        return vm;
    });

    return RestResponse.ok(page);
}

```

分页查询 → map 转 VM → RestResponse

## 上传文件

controller:

```java
@RestController
@RequestMapping("/upload")
public class FileUploadController {

    @Autowired
    private OSS ossClient;

    @Value("${aliyun.oss.bucketName}")
    private String bucketName;

    @Value("${aliyun.oss.endpoint}")
    private String endpoint;

    @PostMapping("/oss")
    public String uploadFile(@RequestParam("file") MultipartFile file) throws IOException {
        // 1. 获取原始文件名
        String originalFilename = file.getOriginalFilename();
        String fileName = UUID.randomUUID().toString() + "-" + originalFilename;

        // 2. 上传文件流到 OSS
        ossClient.putObject(bucketName, fileName, file.getInputStream());

        // 3. 拼接文件访问 URL
        String fileUrl = "https://" + bucketName + "." + endpoint + "/" + fileName;
        return fileUrl;
    }
}

```

application.yml 配置:

```java
aliyun:
  oss:
    endpoint: oss-cn-hangzhou.aliyuncs.com
    accessKeyId: YOUR_ACCESS_KEY_ID
    accessKeySecret: YOUR_ACCESS_KEY_SECRET
    bucketName: your-bucket-name

```

OSSClient 配置类：

```java
@Configuration
public class OssConfig {

    @Value("${aliyun.oss.endpoint}")
    private String endpoint;

    @Value("${aliyun.oss.accessKeyId}")
    private String accessKeyId;

    @Value("${aliyun.oss.accessKeySecret}")
    private String accessKeySecret;

    @Bean
    public OSS ossClient() {
        return new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    }
}

```



# 注解

> 动态参数用 `@PathVariable`，将 URL 中的占位符参数绑定到控制器方法的参数上。比如：
>
> ```java
> @RequestMapping(value = "/subject/select/{id}", method = RequestMethod.POST)
> public RestResponse<SubjectEditRequestVM> select(@PathVariable Integer id)
> 
> ```
>



> `@RequestMapping`，明确 URL 和 HTTP 方法。
>
> ```
> @RequestMapping(value = "/subject/select/{id}", method = RequestMethod.POST)
> public RestResponse<SubjectEditRequestVM> select(@PathVariable Integer id)
> 
> ```
>

> *@RestController*
>
> 等同于 `@Controller` + `@ResponseBody`。
>
> 将该类标记为 Spring MVC 的控制器，并自动将方法返回值序列化为 JSON（或其他格式）写入 HTTP 响应体。

> ## `@RequestBody`
>
> - **作用**：
>   - 将 HTTP 请求体中的 JSON（或其他格式）反序列化为方法参数的 Java 对象。
> - **使用场景**：
>   - 接收 POST、PUT 等请求中传来的 JSON 数据。
>
> ```
> @PostMapping("/edit")
> public RestResponse edit(@RequestBody ExamPaperSubmitVM vm) { ... }
> 
> 
> 
> ```

> ## `@Valid`
>
> - **作用**：
>   - 启用对方法参数（通常与 `@RequestBody` 或表单对象）上的 JSR-303/JSR-380 校验注解（如 `@NotNull`、`@Size`）的校验。
> - **使用场景**：
>   - 当你在 VM 或 DTO 类上使用了校验注解，需要在 Controller 中自动触发校验，并在验证失败时抛出异常。
>
> ```
> 、
> public RestResponse edit(@RequestBody @Valid ExamPaperSubmitVM vm) { ... }
> ```

> ## `@Autowired`
>
> - **作用**：
>   - 将 Spring 容器中的 Bean 自动注入到当前类的字段或构造函数中。
> - **使用场景**：
>   - 在 Controller、Service 等类中注入依赖的 service、repository、publisher 等。
>
> ```
> @Autowired
> public ExamPaperAnswerController(ExamPaperAnswerService examPaperAnswerService, ...) {
>     this.examPaperAnswerService = examPaperAnswerService;
>     ...
> }
> ```

> ## @EventListener / ApplicationEventPublisher）
>
> - **作用**：
>   - **`ApplicationEventPublisher`**：通过 `publishEvent()` 发布自定义事件。
>   - **`@EventListener`**（可选）：在其他 bean 中使用，监听并处理被发布的事件。
> - **使用场景**：
>   - 解耦业务逻辑，通过事件驱动在不同模块间传递消息。
>
> ```
> eventPublisher.publishEvent(new UserEvent(userEventLog));
> ```

> ### `@RequestParam` 
>
> #### 1. 基本用法（绑定查询参数）：
>
> ```java
> @GetMapping("/hello")
> public String hello(@RequestParam String name) {
>     return "Hello " + name;
> }
> ```
>
> 访问 `/hello?name=Tom`，控制器会自动把 `name=Tom` 绑定到方法参数 `name` 上。
>
> #### 2. 设置参数名称和默认值：
>
> ```java
> @GetMapping("/hello")
> public String hello(@RequestParam(value = "name", required = false, defaultValue = "Guest") String name) {
>     return "Hello " + name;
> }
> ```
>
> - `value`：绑定的参数名
> - `required=false`：表示可以不传
> - `defaultValue="Guest"`：如果没传，则使用默认值



# 实体类

## 设计原则

**职责单一**
 每个实体类只负责**映射一张表或一组业务概念数据**，不要在同一个类里混合多种无关数据。

**注解规范**

- 持久层：使用 **MyBatis‑Plus** 时，类上加 `@TableName("...")`，**主键字段加 `@TableId`；**
- JPA：使用 `@Entity`、`@Table`、`@Id` 等；二者二选一，保持全项目一致。

**字段类型**

- 日期／时间：**建议使用 `java.time.LocalDateTime`**，而非过时的 `java.util.Date`；
- 枚举字段：可设计成 `String` 存储 code，再在业务层或实体层加上枚举转换方法。

**Lombok 简化**

- **使用 `@Data` 或者更精细的 `@Getter`/`@Setter`、`@Builder`、`@NoArgsConstructor`/`@AllArgsConstructor`，减少模板代码；**
- 如果需要链式调用，可以加 `@Accessors(chain = true)`。

**字段校验**

- **对于输入层（DTO／VM）使用 `@NotNull`、`@Size` 等进行校验；**
- 实体层一般不加校验注解，保持纯粹的映射。

**编码规范**

- 字段命名：`camelCase`；
- 类命名：`PascalCase`，与表/业务概念一一对应；
- 避免在实体里写过多逻辑方法，仅保持必要的枚举转换、辅助判断。

**公共字段**

若多数表都有 `createTime`、`updateTime`，可抽 BaseEntity：

```java
@Data
public class BaseEntity {
    protected LocalDateTime createTime;
    protected LocalDateTime updateTime;
}
// 然后所有实体 extends BaseEntity

```

然后其他的类继承BaseEntity即可



## 模板

### 数据库表映射

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
@Accessors(chain = true)
@TableName("t_{表名}")  // 对应数据库表名
public class {EntityName} {

    /** 主键 */
    @TableId(type = IdType.ASSIGN_UUID)
    private String id;

    /** 业务编码 */
    private String code;

    /** 状态：OPEN、CLOSED 等 */
    private String status;

    /** 开始时间 */
    private LocalDateTime startTime;

    /** 结束时间 */
    private LocalDateTime endTime;

    // —— 如果字段较多，可分模块写注释，保持可读 —— //
}
```

**@Data**：自动生成 getter/setter、toString、equals、hashCode。

**@Builder + @Accessors(chain = true)**：支持链式构建，更清晰。

**@TableName**：指定表名；**@TableId**：指定主键生成策略。

**LocalDateTime**：最新的 Java 时间 API。

### POJO 对象

```java
 */
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class QuestionObject {

    /** 题干内容 */
    private String titleContent;

    /** 解析说明 */
    private String analyze;

    /** 选项列表 */
    private List<QuestionItemObject> options;

    /** 正确答案 */
    private String correct;
}
```

这种对象不加 ORM 注解，仅加 Lombok。

字段名称尽量自解释；

用 `List<QuestionItemObject>` 等复合类型时，可结合 Jackson 自动序列化。

# redis配合数据库设计

![已生成图片](https://sdmntprnorthcentralus.oaiusercontent.com/files/00000000-d568-622f-8c61-f824a6ab6f98/raw?se=2025-05-03T11%3A13%3A32Z&sp=r&sv=2024-08-04&sr=b&scid=a1fef60e-4329-5554-9696-5612d4382f45&skoid=de76bc29-7017-43d4-8d90-7a49512bae0f&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2025-05-03T10%3A05%3A58Z&ske=2025-05-04T10%3A05%3A58Z&sks=b&skv=2024-08-04&sig=Kx5kBP6rSqaS7XbDWK/6yPbG9FP1xuAikqhMZiJCXz0%3D)

## 配合策略

**1.使用 Hash 存任务信息，避免多次 Redis 请求**

```java
Map<Object,Object> meta = redisTemplate.opsForHash().entries(TASK_PREFIX+taskId);
Map<String, String> meta = new HashMap<>();
meta.put("code", code);
meta.put("status", "Open");
meta.put("start", String.valueOf(startTs));
meta.put("end", String.valueOf(endTs));
redisTemplate.opsForHash().putAll("sign:task:" + taskId, meta);
redisTemplate.expire("sign:task:" + taskId, Duration.between(LocalDateTime.now(), end));

```

避免多个 Redis key；

`sign()` 时一次读取全部字段，无需多次调用；

更加一致和规范。

**2.异步 + 队列方式持久化**

**定时任务**写回（适合非实时要求）

- 每分钟、每 5 分钟定时从 Redis 中读取新增数据批量写入 MySQL；
- 可结合 Bitmap/ZSet 等结构做更多统计。

**使用 Redis Stream 或 MQ 异步写回**

```java
redisTemplate.opsForStream().add("stream:sign-record", Map.of("taskId", taskId, "studentId", studentId));

```

异步消费者从中读取写入数据库。

这样加快请求的返回时间，不影响主线程的工作

**3.高并发保护：加锁 + 幂等校验**

使用 `isMember()` 防止重复签到；

高并发场景下，仍可能发生并发写 Redis；

可加上 Redisson 分布式锁或 Lua 脚本实现原子性：

```java
-- Lua 示例：只有未签到时才添加，并返回 true
if redis.call("SISMEMBER", KEYS[1], ARGV[1]) == 0 then
    redis.call("SADD", KEYS[1], ARGV[1])
    return 1
else
    return 0
end

```

**4.缓存失效策略**

所有 Redis 数据都应设置过期时间：

- 任务缓存：根据 `endTime` 动态设置；
- 签到记录：可设置为任务结束后几天；

防止 Redis 缓存积压，资源不释放。

比如验证码可以使用这个策略，设置验证码的过期时间





