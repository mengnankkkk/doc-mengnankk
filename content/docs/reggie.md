---
title: 瑞吉外卖解析
categories:
  - java
  - 项目
  
abbrlink: 2701
date: 2024.10.18
tags: 
   - spring boot
   - java
---

# 公共字段解析

在使用的时候，有一些字段是一直在使用的。我们为了方便会使用MyBatis-Plus 提供的注解来完成这个功能

```
 /** 创建时间 *公共字段要进行加注解/
    @TableField(fill = FieldFill.INSERT) // 自动填充，插入时自动填充
    private LocalDateTime createTime;

    /** 更新时间 */
    @TableField(fill = FieldFill.INSERT_UPDATE) // 自动填充，插入和更新时自动填充
    private LocalDateTime updateTime;

    @TableField(fill = FieldFill.INSERT) // 自动填充，插入时自动填充
    private Long createUser;

    @TableField(fill = FieldFill.INSERT_UPDATE) // 自动填充，插入和更新时自动填充
    private Long updateUser;
```

比如这四个是经常使用的公共字段

给他们加上注解

```
@TableField(fill = FieldFill.INSERT)
```

前面是注解，后面fill后面的是文件填充的方式

```
public enum FieldFill {
    DEFAULT,
    INSERT,
    UPDATE,
    INSERT_UPDATE;

    private FieldFill() {
    }
}
```

我们看源码就会发现他有四种填充的方式

**`DEFAULT`**: 不进行自动填充。

**`INSERT`**: 仅在插入时自动填充。

**`UPDATE`**: 仅在更新时自动填充。

**`INSERT_UPDATE`**: 在插入和更新时都会自动填充。

然后我们在common包下加入一个类用来实现这个

```
@Slf4j
@Component
```

使用这两个注解

分别使用日志和Spring的注解

分别实现两个方法

一个是insertFill

一个是updateFill

```
public void insertFill(MetaObject metaObject) {
        log.info("公共字段自动填充[insert]...");
        log.info(metaObject.toString());
        metaObject.setValue("createTime", LocalDateTime.now());
        metaObject.setValue("updateTime", LocalDateTime.now());
        metaObject.setValue("createUser", BaseContext.getCurrentId());     // 从 ThreadLocal 中获取当前用户的 id
        metaObject.setValue("updateUser", BaseContext.getCurrentId());     // 从 ThreadLocal 中获取当前用户的 id
    }
```

```
public void updateFill(MetaObject metaObject) {
        log.info("公共字段自动填充[update]...");
        log.info(metaObject.toString());

        long id = Thread.currentThread().getId();                            // 获取当前线程的id
        log.info("当前线程id={}", id);                                        // Slf4j的日志输出

        metaObject.setValue("updateTime", LocalDateTime.now());
        metaObject.setValue("updateUser", BaseContext.getCurrentId());  // 从ThreadLocal中获取当前线程的用户id
    }
```

# 菜品分类解析

```
@RestController
@RequestMapping("/category")
@Slf4j
public class CategoryController {
    @Autowired
    private CategoryService categoryService;
    @PostMapping
    public R<String> save(@RequestBody Category category){
        log.info("category:{}",category);
        categoryService.save(category);
        return R.success("添加分类成功");
    }
    @PostMapping("/page")
    public R<Page<Category>> page(int page, int pageSize) {
        Page<Category> categoryPage = new Page<>(page, pageSize);
        LambdaQueryWrapper<Category> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.orderByAsc(Category::getSort);

        // 通过实例调用 page 方法
        categoryService.page(categoryPage, queryWrapper);

        return R.success(categoryPage);
    }
```

要做一个新的Controller

必须有一个类，然后里面定义各种数据

然后有一个Serivice接口

一个Serviceimpl类

一个Mapper接口

然后在Controller 里写具体的方法

## 1.添加分类

先设计一个Api请求的方法

然后定义

**`@Autowired private CategoryService categoryService;`**：使用 `@Autowired` 注解，将 `CategoryService` 服务自动注入到控制器中。`CategoryService` 包含保存 `Category` 对象的逻辑。

然后开始写具体的方法，save作为添加方法

```
 public R<String> save(@RequestBody Category category)
```

**`@PostMapping`**：标注该方法为 HTTP POST 请求的映射。客户端通过发送 POST 请求来触发该方法。就是接受浏览器的post请求

**`public R<String> save`**：方法返回类型为 `R<String>`，其中 `R` 是一个泛型响应包装类，返回一个包含字符串信息的响应。

**`@RequestBody Category category`**：使用 `@RequestBody` 注解，将请求中的 JSON 数据反序列化为一个 `Category` 对象，作为 `save` 方法的参数。

```
log.info("category:{}",category);
        categoryService.save(category);
        return R.success("添加分类成功");
```

首先是日志提示

然后**`categoryService.save(category);`**：调用 `CategoryService` 的 `save` 方法，将 `category` 对象存储到数据库中。这个 `save` 方法通常由 MyBatis-Plus 框架提供，`CategoryService` 通过继承 `IService<Category>` 或实现相关接口来提供该方法。这个需要在接口的时候继承

然后返回响应

```
return R.success("添加分类成功");
```

总体代码

```
@PostMapping
    public R<String> save(@RequestBody Category category){
        log.info("category:{}",category);
        categoryService.save(category);
        return R.success("添加分类成功");
    }
```



## 2.分页查询

```
@PostMapping("/page")
```

接受post请求，然后url栏中做出响应的回应

```
public R<Page<Category>> page(int page, int pageSize)
```

定义page方法

里面加入page和pageSize参数，注意要使用大写

```
Page<Category> categoryPage = new Page<>(page, pageSize);
```

新建一个Page构造器

```
LambdaQueryWrapper<Category> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.orderByAsc(Category::getSort);
```

这里面是条件过滤，按照升序进行排序

```
// 通过实例调用 page 方法
        categoryService.page(categoryPage, queryWrapper);
```

调用接口的page方法

```
return R.success(categoryPage);
```

返回数据

一般的分页查询都是这样做的

例如：

之前的Employee

```
@GetMapping("/page")
    public R<Page<Employee>> page(int page, int pageSize, String name) {
        log.info("分页查询，page={}, pageSize={}, name={}", page, pageSize, name);

        // 分页构造器
        com.baomidou.mybatisplus.extension.plugins.pagination.Page<Employee> pageInfo = new Page<>(page, pageSize);

        // 条件构造器
        LambdaQueryWrapper<Employee> queryWrapper = new LambdaQueryWrapper<>();
        if (StringUtils.hasText(name)) {  // 修正为 hasText 以避免空白字符串的情况
            queryWrapper.like(Employee::getName, name);
        }
        queryWrapper.orderByDesc(Employee::getUpdateTime);

        // 执行查询
        employeeService.page(pageInfo, queryWrapper);

        // 返回分页结果
        return R.success(pageInfo);
    }
```

差不多都是一个模板

## 3.删除分类

首先还是和之前的controller一样，先新建mapper 和Service和ServiceImpl

因为需要检测是不是和其他的东西（例如菜品，菜单有关联）所以他们的mapper和Service和Serviceimpl也需要新建

然后controller调用的时候需要一个remove方法

所以在CategoryService下需要实现一个

```
ublic void remove(Long id);
```

remove方法

在CategoryServiceimpl写具体的remove方法

```
@Autowired
    private DishService dishService;
    @Autowired
    private SetmealService setmealService;

    /**
     * @param id
     */
    @Override
    public void remove(Long id) {
        LambdaQueryWrapper<Dish> dishLambdaQueryWrapper = new LambdaQueryWrapper<>();
        dishLambdaQueryWrapper.eq(Dish::getCategoryId,id);
        int count1 =  dishService.count(dishLambdaQueryWrapper);
        if (count1>0){
            throw new CustomException("当前分类下关联了菜品，不能删除");

        }
        LambdaQueryWrapper<Setmeal> setmealLambdaQueryWrapper = new LambdaQueryWrapper<>();
        setmealLambdaQueryWrapper.eq(Setmeal::getCategoryId,id);
        int count2 = setmealService.count();
        if (count2>0){
            throw new CustomException("当前分类下关联了套餐，不能删除");

        }
        super.removeById(id);
    }
```

先新建remove方法

```
LambdaQueryWrapper<Dish> dishLambdaQueryWrapper = new LambdaQueryWrapper<>();
```

条件过滤器

```
dishLambdaQueryWrapper.eq(Dish::getCategoryId,id);
```

通过等于id来过滤

```
int count1 =  dishService.count(dishLambdaQueryWrapper);
```

然后记录次数

```
if (count1>0){
            throw new CustomException("当前分类下关联了菜品，不能删除");

        }
```

非0的时候不能删除

然后下面的相似

```
LambdaQueryWrapper<Setmeal> setmealLambdaQueryWrapper = new LambdaQueryWrapper<>();
        setmealLambdaQueryWrapper.eq(Setmeal::getCategoryId,id);
        int count2 = setmealService.count();
        if (count2>0){
            throw new CustomException("当前分类下关联了套餐，不能删除");

        }
```

最后如果上面的都没成立的话

```
super.removeById(id);
```

再去调用父类的remove方法，就是mybatisplus所提供的removeById的方法

然后最后在controller里面

```
 @DeleteMapping//注意是ids哦
    public R<String> delete(Long ids){
        log.info("删除id为{}",ids);
        categoryService.remove(ids);
        return R.success("分类信息删除成功");

    }
```

使用上面自定义的remove方法

# 文件上传和下载

文件上传和下载是一个重要的功能

直接创建一个commonController.java

然后注解为

```
@RestController
@RequestMapping("/common")
@Slf4j
```

代表是controller

然后页面为./common

日志

然后导入文件地址

```
 @Value("${reggie.path}")
    private String basePath;
```

注意@value是spring包下面的

```
reggie:
  # 文件存储位置信息|必填
  path: C:\Users\river\code\work\javaProjects\rikky-takeaway\img\
```

application配置

## upload方法

然后开始写upload方法

```
@PostMapping("/upload")
    public R<String> upload(MultipartFile file) {
        log.info(file.toString());

        String orginname = file.getOriginalFilename();//原始文件名
        String suffix = orginname.substring(orginname.lastIndexOf("."));
        String uuidname = UUID.randomUUID().toString()+suffix;

        File dir  = new File(basePath);
        if (!dir.exists()){
            dir.mkdirs();
        }

        try {
            file.transferTo(new File(basePath+uuidname));
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        return R.success(uuidname);
    }
```

文件上传在浏览器中以post请求来实现

所以注解为postMapping

```
String orginname = file.getOriginalFilename();//原始文件名
String suffix = orginname.substring(orginname.lastIndexOf("."));
String uuidname = UUID.randomUUID().toString()+suffix;
```

suffix代表 的是文件的后缀名

uuid生成一个uuid



```
File dir  = new File(basePath);
if (!dir.exists()){
    dir.mkdirs(
}
```

判断一下是否存在

不存在创建

]

```
 file.transferTo(new File(basePath+uuidname));
```

最后上传，为地址加上uuid的Name

然后返回

```
return R.success(uuidname);
```

## download方法

```
@GetMapping("/download")
    public void download(String name, HttpServletResponse response) {
        try (FileInputStream fileInputStream = new FileInputStream(new File(basePath + name));
             ServletOutputStream servletOutputStream = response.getOutputStream()) {

            response.setContentType("image/jpeg");
            byte[] bytes = new byte[1024];
            int len;

            while ((len = fileInputStream.read(bytes)) != -1) {
                servletOutputStream.write(bytes, 0, len);
            }

            servletOutputStream.flush();
        } catch (FileNotFoundException e) {
            // 文件未找到的处理逻辑
            response.setStatus(HttpServletResponse.SC_NOT_FOUND);
            log.error("File not found: " + name, e);
        } catch (IOException e) {
            // 其他I/O异常处理
            response.setStatus(HttpServletResponse.SC_INTERNAL_SERVER_ERROR);
            log.error("Error occurred during file download", e);
        }
    }
```

download就是get方法了

然后

```
FileInputStream fileInputStream = new FileInputStream(new File(basePath + name));
             ServletOutputStream servletOutputStream = response.getOutputStream())
```

去读取这个文件

设置类型

```
response.setContentType("image/jpeg");
```

```
byte[] bytes = new byte[1024];
            int len;

            while ((len = fileInputStream.read(bytes)) != -1) {
                servletOutputStream.write(bytes, 0, len);
            }
```

去讲文件内的内容给读取出来

```
servletOutputStream.flush();
```

刷新流

```
catch (FileNotFoundException e) {
            // 文件未找到的处理逻辑
            response.setStatus(HttpServletResponse.SC_NOT_FOUND);
            log.error("File not found: " + name, e);
        } catch (IOException e) {
            // 其他I/O异常处理
            response.setStatus(HttpServletResponse.SC_INTERNAL_SERVER_ERROR);
            log.error("Error occurred during file download", e);
```

异常处理

使用这个结构的时候，流会自动关闭所以不用了

# 新建菜品

还是跟之前的一样，先新建Mapping，然后新建Service，然后再新建ServiceImpl

最后写Controller

## 所选分类

这个和分类有关，所以放在Categorycontroller下面

```
/**
 * 根据条件查询数据
 * @param category
 * @return
 */
@GetMapping("/list")
public R<List<Category>> list(Category category){
    LambdaQueryWrapper<Category> lambdaQueryWrapper = new LambdaQueryWrapper<>();
    lambdaQueryWrapper.eq(category.getType()!=null,Category::getType,category.getType());
    lambdaQueryWrapper.orderByAsc(Category::getSort).orderByDesc(Category::getUpdateTime);
    categoryService.list(lambdaQueryWrapper);
    List<Category> list = categoryService.list(lambdaQueryWrapper);

    return R.success(list);

}
```

```
@GetMapping("/list")
```

和前端像对应



```
LambdaQueryWrapper<Category> lambdaQueryWrapper = new LambdaQueryWrapper<>();
lambdaQueryWrapper.eq(category.getType()!=null,Category::getType,category.getType());
lambdaQueryWrapper.orderByAsc(Category::getSort).orderByDesc(Category::getUpdateTime);
```

条件过滤一下啊

瞎看type是不是等于空，然后按sort来升序，再按更新时间来降序排列

```
categoryService.list(lambdaQueryWrapper);
```

然后Service列出

```
List<Category> list = categoryService.list(lambdaQueryWrapper);

        return R.success(list);
```

把结果放List里面然后返回结果

## 提交数据

想要把前端所写的数据提交到后端，对数据库进行更改就要将其用json格式提交

就要导入dto类型

在项目的包下新建dto包，然后导入类

```
package com.mengnankk.dto;


import com.mengnankk.entity.Dish;
import com.mengnankk.entity.DishFlavor;
import lombok.Data;
import java.util.ArrayList;
import java.util.List;


@Data
public class DishDto extends Dish {

    private List<DishFlavor> flavors = new ArrayList<>();

    private String categoryName;

    private Integer copies;
}

```

因为是新增菜品，所以要在DishController下新增方法

```
@Slf4j
@RestController
@RequestMapping("/dish")
public class DishController {
    @Autowired
    private DishService dishService;

    @Autowired
    private DishFlavorServiceImpl dishFlavorService;

@PostMapping
    public R<String> save(@RequestBody DishDto dishDto){
        log.info(dishDto.toString());

        return R.success("新增成功");
    }
```

新增save方法，将其转为类型后回显

然后还涉及到菜品的口味，所以DishFlavor相关的Service和ServiceImpl都要建立对应的实现

Service建立

```
public interface DishFlavorService extends IService<DishFlavor> {
    // 保存菜品及其口味
    R<String> saveWithFlavor(DishDto dishDto);
}
```

然后Impl进行具体方法的实现

```
@Autowired
    private DishService dishService;  // 注入 DishService 用于保存菜品

    @Override
    public R<String> saveWithFlavor(DishDto dishDto) {
        log.info("开始录入信息");
        // 1. 保存菜品信息
        Dish dish = new Dish();
        // 假设 DishDto 中有名称、类别等属性，复制到 Dish 实体
        dish.setName(dishDto.getName());
        dish.setCategoryId(dishDto.getCategoryId());
        // 其他属性的复制...

        // 保存菜品实体
        dishService.save(dish);  // 调用 DishService 保存菜品

        // 2. 获取保存后的菜品 ID
        Long dishID = dish.getId();
        if (dishID == null) {
            throw new RuntimeException("保存菜品时未能生成 ID");
        }

        // 3. 保存口味数据
        List<DishFlavor> flavors = dishDto.getFlavors().stream().map(item -> {
            item.setDishId(dishID);  // 设置每个口味的菜品 ID
            return item;
        }).collect(Collectors.toList());

        // 批量保存口味
        this.saveBatch(flavors);
        return R.success("新增成功");
    }
```

具体的注释上都写了

然后这项功能算是这样就写完了
