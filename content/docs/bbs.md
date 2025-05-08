---
title:  bbs sql
categories:
  - sql
  
abbrlink: 2701
date: 2024.06.6
tags: 
   - sql
swiper_index: 2


---



## 数据库

根据实际情况选择数据库，选择的是mysql version 为 8.0.37 MySQL Community Server

在resources下application.properties文件里规定服务器的各类配置

#### 服务器配置

```
properties复制代码server.port=8080
spring.thymeleaf.cache=false
```

- `server.port=8080`：指定Spring Boot应用运行的端口号为8080。
- `spring.thymeleaf.cache=false`：禁用Thymeleaf模板的缓存，以便在开发过程中实时查看更改效果。

#### 数据源配置

```
properties复制代码spring.datasource.name=my-bbs-datasource
spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/my_bbs_db?useUnicode=true&serverTimezone=Asia/Shanghai&characterEncoding=utf8&autoReconnect=true&useSSL=false&allowMultiQueries=true
spring.datasource.username=Margit
spring.datasource.password=8750613a
```

- `spring.datasource.name=my-bbs-datasource`：数据源的名称。
- `spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver`：指定MySQL JDBC驱动。
- `spring.datasource.url=jdbc:mysql://localhost:3306/my_bbs_db?useUnicode=true&serverTimezone=Asia/Shanghai&characterEncoding=utf8&autoReconnect=true&useSSL=false&allowMultiQueries=true`：数据库连接的URL，包含了字符编码、时区、SSL等配置。
- `spring.datasource.username=Margit`：数据库用户名。
- `spring.datasource.password=8750613a`：数据库密码。

#### HikariCP连接池配置

```
properties复制代码spring.datasource.hikari.minimum-idle=5
spring.datasource.hikari.maximum-pool-size=15
spring.datasource.hikari.auto-commit=true
spring.datasource.hikari.idle-timeout=60000
spring.datasource.hikari.pool-name=hikariCP
spring.datasource.hikari.max-lifetime=600000
spring.datasource.hikari.connection-timeout=30000
spring.datasource.hikari.connection-test-query=SELECT 1
```

- `spring.datasource.hikari.minimum-idle=5`：连接池中最小空闲连接数。
- `spring.datasource.hikari.maximum-pool-size=15`：连接池中最大连接数。
- `spring.datasource.hikari.auto-commit=true`：设置自动提交。
- `spring.datasource.hikari.idle-timeout=60000`：连接空闲时间（毫秒），超过该时间连接将被释放。
- `spring.datasource.hikari.pool-name=hikariCP`：连接池名称。
- `spring.datasource.hikari.max-lifetime=600000`：连接的最大生命周期（毫秒），超过该时间连接将被关闭。
- `spring.datasource.hikari.connection-timeout=30000`：等待连接的最大时间（毫秒）。
- `spring.datasource.hikari.connection-test-query=SELECT 1`：测试连接的SQL查询。

#### MyBatis配置

```
properties
复制代码
mybatis.mapper-locations=classpath:mapper/*Mapper.xml
```

- `mybatis.mapper-locations=classpath:mapper/*Mapper.xml`：指定MyBatis的Mapper文件位置。

#### 日志配置

```
properties
复制代码
logging.level.com.my.bbs.dao=debug
```

- `logging.level.com.my.bbs.dao=debug`：设置包`com.my.bbs.dao`下的日志级别为`debug`，便于调试。

确保数据安全和防止数据丢失，要进行date的备份，使用`mysqldump`工具备份MySQL数据库：

```
sh复制代码# 备份整个数据库
mysqldump -u username -p database_name > /path/to/backup/backup.sql

# 备份所有数据库
mysqldump -u username -p --all-databases > /path/to/backup/all_databases.sql
```

### 

#### 管理工具

在管理数据库的选择上，选择的是Navicat，可以有效的管理数据库。

#### sql文件

**包含了创建名为`tb_bbs_post`的MySQL表的结构。这个表用于存储论坛帖子的相关信息。每个帖子有一个唯一的主键`post_id`，以及发布者ID、帖子标题、内容等字段。**

**创建了名为tb_bbs_post的mysql表，用于存储帖子user的相关内容：**

- **存储用户信息，包括登录名、密码、昵称、头像等。**
- **每个用户有一个唯一的主键`user_id`。**
- **`login_name`字段用于用户登录，`password_md5`字段存储密码的MD5加密值。**
- **`user_status`字段表示用户状态，0表示正常，1表示禁言；`is_admin`字段表示用户是否为管理员。**
- **`last_login_time`字段记录用户最后登录时间，`create_time`字段记录用户注册时间。**

**创建了名为tb_bbs_post的mysql表，用于存储帖子的post的内容**

`tb_post_category`：

- **存储帖子分类信息，包括分类名称和排序值。**

- **每个分类有一个唯一的主键`category_id`。**

- **`category_name`字段存储分类名称，`category_rank`字段表示分类的排序值。**

- **`is_deleted`字段表示分类是否被删除，0表示未删除，1表示已删除。**

- **`create_time`字段记录分类创建时间。**

- `tb_post_collect_record`：

  - **存储帖子收藏记录，包括帖子ID和用户ID。**
  - **每条记录有一个唯一的主键`record_id`。**
  - **`post_id`字段表示收藏的帖子ID，`user_id`字段表示收藏者的用户ID。**
  - **`create_time`字段记录收藏记录的创建时间。**

  `tb_post_comment`：

  - **存储帖子评论信息，包括帖子ID、评论者ID、评论内容等。**
  - **每条评论有一个唯一的主键`comment_id`。**
  - **`post_id`字段表示评论所属的帖子ID，`comment_user_id`字段表示评论者的用户ID。**
  - **`comment_body`字段存储评论内容，`parent_comment_user_id`字段表示所回复的上一级评论的用户ID。**
  - `comment_create_time`字段记录评论的创建时间，`is_deleted`字段表示评论是否被删除。

#### 交互

my_bbs_dbMapper.xml文件用来定义与数据库交互的SQL映射

其中的这个`resultMap`定义了数据库表到Java对象的映射关系。每个`<result>`标签指定了数据库列（`column`）与Java对象属性（`property`）之间的映射关系，以及列的JDBC类型（`jdbcType`）。这个`resultMap`扩展了`BaseResultMap`，添加了对大文本（BLOB）的支持，映射了`post_content`列。这个SQL片段定义了一个动态生成的`WHERE`子句，用于根据条件动态构建查询语句。`<foreach>`、`<if>`、`<choose>`等标签用来遍历条件并生成相应的SQL语句。这个SQL片段与`Example_Where_Clause`类似，但用于更新操作中的`WHERE`子句。`Base_Column_List` 和 `Blob_Column_List` 定义了基础列和BLOB列的列表，用于复用。

`selectByExampleWithBLOBs` 和 `selectByExample` 定义了查询方法，分别用于查询包含BLOB列和不包含BLOB列的数据。

这些查询方法根据参数动态生成SQL语句，包括`distinct`、`WHERE`子句和`ORDER BY`子句。

- `selectByExample` 和 `selectByPrimaryKey` 定义了查询方法，分别用于根据条件和主键查询数据。
- `deleteByPrimaryKey` 和 `deleteByExample` 定义了删除方法，分别用于根据主键和条件删除数据。
- `insert` 和 `insertSelective` 定义了插入方法，分别用于插入完整记录和选择性插入记录。
- `countByExample` 用于根据条件统计记录数量。

这些SQL映射定义了与数据库表`tb_bbs_post`的基本CRUD操作，利用MyBatis动态生成SQL语句，提高了代码的灵活性和可维护性。

`countByExample` 用于根据条件统计记录数量。

`updateByExampleSelective` 用于根据条件选择性地更新记录。

`updateByExampleWithBLOBs` 和 `updateByExample` 用于根据条件更新记录，分别包括和不包括BLOB字段。

`updateByPrimaryKeySelective` 和 `updateByPrimaryKeyWithBLOBs` 用于根据主键更新记录，分别选择性更新和更新所有字段。`update`方法更新所有字段，包括`post_content`。

`updateByPrimaryKey`方法更新所有字段，但不包括`post_content`。

这两个更新操作都是根据主键`post_id`进行的。

在这个文件中mybatisGeneratorConfig.xml用于生成与MyBatis ORM框架一起使用的Java对象和Mapper文件。根节点

```
xml
复制代码
<generatorConfiguration>
```

- 表示MyBatis Generator的配置文件开始。

```
xml
复制代码
<context id="DB2Tables" targetRuntime="MyBatis3">
```

- `id`：上下文的标识符。
- `targetRuntime`：指定生成代码的运行时环境，`MyBatis3`表示生成MyBatis 3.x兼容代码。

##### 数据库连接配置

```
xml复制代码<jdbcConnection driverClass="com.mysql.jdbc.Driver"
    connectionURL="jdbc:mysql://localhost:3306/my_bbs_db?serverTimezone=UTC"
    userId="mengnankk"
    password="Zyk2215290444">
</jdbcConnection>
```

- `driverClass`：JDBC驱动类名。
- `connectionURL`：数据库连接URL。
- `userId`：数据库用户名。
- `password`：数据库密码。

##### Java类型解析器

```
xml复制代码<javaTypeResolver>
    <property name="forceBigDecimals" value="false"/>
</javaTypeResolver>
```

- 用于解析数据库类型到Java类型。
- `forceBigDecimals`：设置为`false`表示不强制使用`BigDecimal`类型。

##### Java模型生成器

```
xml复制代码<javaModelGenerator targetPackage="com.my.bbs"
                    targetProject="src/main/java">
    <property name="enableSubPackages" value="true"/>
    <property name="trimStrings" value="true"/>
</javaModelGenerator>
```

- `targetPackage`：生成的模型类包名。
- `targetProject`：生成的模型类文件所在的项目路径。
- `enableSubPackages`：设置为`true`表示支持子包。
- `trimStrings`：设置为`true`表示自动去除字符串两端的空格。

##### SQL映射文件生成器

```
xml复制代码<sqlMapGenerator targetPackage="com.my.bbs"
                 targetProject="src/main/resources">
    <property name="enableSubPackages" value="true"/>
</sqlMapGenerator>
```

- `targetPackage`：生成的SQL映射文件包名。
- `targetProject`：生成的SQL映射文件所在的项目路径。
- `enableSubPackages`：设置为`true`表示支持子包。

##### Java客户端生成器

```
xml复制代码<javaClientGenerator type="XMLMAPPER"
                     targetPackage="com.my.bbs"
                     targetProject="src/main/java">
    <property name="enableSubPackages" value="true"/>
</javaClientGenerator>
```

- `type`：指定生成的Java客户端类型，`XMLMAPPER`表示生成XML映射文件的Mapper接口。
- `targetPackage`：生成的Mapper接口包名。
- `targetProject`：生成的Mapper接口所在的项目路径。
- `enableSubPackages`：设置为`true`表示支持子包。

##### 指定生成的表及对应的实体类

```
xml复制代码<table tableName="tb_bbs_user" domainObjectName="my_bbs_db"/>
<table tableName="tb_bbs_post" domainObjectName="my_bbs_db"/>
```

- `tableName`：指定数据库中的表名。
- `domainObjectName`：指定生成的实体类名。

这个配置文件通过指定数据库连接、模型生成器、SQL映射文件生成器和Java客户端生成器等信息，生成与`tb_bbs_user`和`tb_bbs_post`表相关的Java对象和MyBatis Mapper文件。你可以添加更多的`<table>`节点来生成其他表的映射。

#### datemapper

##### BBSPostCategory

###### 根节点

```
xml
复制代码
<mapper namespace="com.my.bbs.dao.BBSPostCategoryMapper">
```

- 定义了该Mapper文件的命名空间，通常是对应的DAO接口的全限定名。

###### ResultMap

```
xml复制代码<resultMap id="BaseResultMap" type="com.my.bbs.entity.BBSPostCategory">
    <id column="category_id" jdbcType="INTEGER" property="categoryId" />
    <result column="category_name" jdbcType="VARCHAR" property="categoryName" />
    <result column="category_rank" jdbcType="INTEGER" property="categoryRank" />
    <result column="is_deleted" jdbcType="TINYINT" property="isDeleted" />
    <result column="create_time" jdbcType="TIMESTAMP" property="createTime" />
</resultMap>
```

- 定义了`BBSPostCategory`实体类与数据库表`tb_post_category`的映射关系。

###### SQL片段

```
xml复制代码<sql id="Base_Column_List">
    category_id, category_name, category_rank, is_deleted, create_time
</sql>
```

- 定义了一个可重用的SQL片段，包含所有的列名。

###### 

###### 根据主键查询

```
xml复制代码<select id="selectByPrimaryKey" parameterType="java.lang.Integer" resultMap="BaseResultMap">
    select 
    <include refid="Base_Column_List" />
    from tb_post_category
    where category_id = #{categoryId,jdbcType=INTEGER}
</select>
```

- 根据`category_id`查询对应的记录。

###### 查询所有未删除的类别

```
xml复制代码<select id="getBBSPostCategories" resultMap="BaseResultMap">
    select
    <include refid="Base_Column_List" />
    from tb_post_category
    where is_deleted = 0 order by category_rank desc
</select>
```

- 查询所有`is_deleted`字段为`0`的记录，并按`category_rank`降序排列。

###### 删除操作

```
xml复制代码<delete id="deleteByPrimaryKey" parameterType="java.lang.Integer">
    delete from tb_post_category
    where category_id = #{categoryId,jdbcType=INTEGER}
</delete>
```

- 根据`category_id`删除对应的记录。

###### 全字段插入

```
xml复制代码<insert id="insert" parameterType="com.my.bbs.entity.BBSPostCategory">
    insert into tb_post_category (category_id, category_name, category_rank, 
      is_deleted, create_time)
    values (#{categoryId,jdbcType=INTEGER}, #{categoryName,jdbcType=VARCHAR}, #{categoryRank,jdbcType=INTEGER}, 
      #{isDeleted,jdbcType=TINYINT}, #{createTime,jdbcType=TIMESTAMP})
</insert>
```

- 插入一条新的记录，所有字段都必须提供值。

###### 选择性插入

```
xml复制代码<insert id="insertSelective" parameterType="com.my.bbs.entity.BBSPostCategory">
    insert into tb_post_category
    <trim prefix="(" suffix=")" suffixOverrides=",">
      <if test="categoryId != null">
        category_id,
      </if>
      <if test="categoryName != null">
        category_name,
      </if>
      <if test="categoryRank != null">
        category_rank,
      </if>
      <if test="isDeleted != null">
        is_deleted,
      </if>
      <if test="createTime != null">
        create_time,
      </if>
    </trim>
    <trim prefix="values (" suffix=")" suffixOverrides=",">
      <if test="categoryId != null">
        #{categoryId,jdbcType=INTEGER},
      </if>
      <if test="categoryName != null">
        #{categoryName,jdbcType=VARCHAR},
      </if>
      <if test="categoryRank != null">
        #{categoryRank,jdbcType=INTEGER},
      </if>
      <if test="isDeleted != null">
        #{isDeleted,jdbcType=TINYINT},
      </if>
      <if test="createTime != null">
        #{createTime,jdbcType=TIMESTAMP},
      </if>
    </trim>
  </insert>
```

- 仅插入有值的字段。

###### 

###### 选择性更新

```
xml复制代码<update id="updateByPrimaryKeySelective" parameterType="com.my.bbs.entity.BBSPostCategory">
    update tb_post_category
    <set>
      <if test="categoryName != null">
        category_name = #{categoryName,jdbcType=VARCHAR},
      </if>
      <if test="categoryRank != null">
        category_rank = #{categoryRank,jdbcType=INTEGER},
      </if>
      <if test="isDeleted != null">
        is_deleted = #{isDeleted,jdbcType=TINYINT},
      </if>
      <if test="createTime != null">
        create_time = #{createTime,jdbcType=TIMESTAMP},
      </if>
    </set>
    where category_id = #{categoryId,jdbcType=INTEGER}
  </update>
```

- 仅更新有值的字段。

###### 全字段更新

```
xml复制代码<update id="updateByPrimaryKey" parameterType="com.my.bbs.entity.BBSPostCategory">
    update tb_post_category
    set category_name = #{categoryName,jdbcType=VARCHAR},
      category_rank = #{categoryRank,jdbcType=INTEGER},
      is_deleted = #{isDeleted,jdbcType=TINYINT},
      create_time = #{createTime,jdbcType=TIMESTAMP}
    where category_id = #{categoryId,jdbcType=INTEGER}
  </update>
```

#### MyBatis Mapper

好的，我们直接来分析这个MyBatis Mapper文件的功能和结构，而不再打印具体代码。

1. **ResultMap**:
   - 定义了实体类与数据库表的映射关系。`BaseResultMap` 映射 `record_id`, `post_id`, `user_id`, `create_time` 到实体类的相应属性。

2. **Base_Column_List**:
   - 定义了表中的列，以便在SQL查询中复用，减少重复代码。

3. **selectByPrimaryKey**:
   - 通过主键 `record_id` 查询记录，返回单条记录映射到 `BBSPostCollect` 实体类。

4. **selectByUserIdAndPostId**:
   - 根据 `user_id` 和 `post_id` 查询单条记录，用于检查特定用户是否收藏了特定帖子。

5. **listByUserId**:
   - 根据 `user_id` 查询用户收藏的所有帖子，返回结果列表映射到 `BBSPostCollect` 实体类。

6. **deleteByPrimaryKey**:
   - 根据主键 `record_id` 删除记录。

7. **insert**:
   - 插入一条新的收藏记录，所有字段必须有值。

8. **insertSelective**:
   - 插入一条新的收藏记录，允许部分字段为空，只插入非空字段。

9. **updateByPrimaryKeySelective**:
   - 更新一条记录，根据主键 `record_id` 更新非空字段。

10. **updateByPrimaryKey**:
    - 更新一条记录，根据主键 `record_id` 更新所有字段。

这个MyBatis Mapper文件定义了与 `BBSPostCollect` 实体类相关的数据库操作，具体包括查询、插入、更新和删除操作。它使用了 `ResultMap` 映射数据库表 `tb_post_collect_record` 的列与实体类属性之间的关系，并通过 `Base_Column_List` 片段复用列定义。

主要功能包括：
- **selectByPrimaryKey**：根据主键 `record_id` 查询记录。
- **selectByUserIdAndPostId**：根据 `user_id` 和 `post_id` 查询单条记录，用于检查特定用户是否收藏了特定帖子。
- **listByUserId**：根据 `user_id` 查询用户收藏的所有帖子。
- **deleteByPrimaryKey**：根据主键 `record_id` 删除记录。
- **insert**：插入一条新的收藏记录，所有字段必须有值。
- **insertSelective**：插入一条新的收藏记录，允许部分字段为空。
- **updateByPrimaryKeySelective**：根据主键 `record_id` 更新非空字段。
- **updateByPrimaryKey**：根据主键 `record_id` 更新所有字段。

通过这些定义，可以实现对 `tb_post_collect_record` 表的基本CRUD操作，并且可以根据不同的条件进行查询和更新。

用于帖子评论相关的数据库操作的 MyBatis XML 配置文件。它包括了查询帖子评论列表、获取特定用户最近的评论列表、统计评论总数以及插入、更新和删除评论等功能。XML 文件中的各个元素定义了不同的数据库操作，通过 SQL 查询语句和参数映射关系来实现这些功能。



用于帖子评论相关的数据库操作的 MyBatis XML 配置文件。它包括了查询帖子评论列表、获取特定用户最近的评论列表、统计评论总数以及插入、更新和删除评论等功能。XML 文件中的各个元素定义了不同的数据库操作，通过 SQL 查询语句和参数映射关系来实现这些功能。

#### BBSPostCommentMapper.xml

用于帖子评论相关的数据库操作的 MyBatis XML 配置文件。它包括了查询帖子评论列表、获取特定用户最近的评论列表、统计评论总数以及插入、更新和删除评论等功能。XML 文件中的各个元素定义了不同的数据库操作，通过 SQL 查询语句和参数映射关系来实现这些功能。

#### BBSPostMapper.xml

这个 XML 文件是用于 MyBatis 的数据库映射配置，它定义了与帖子相关的数据库操作，主要包括：

- **结果映射（ResultMap）**：定义了两个结果映射，`BaseResultMap` 和 `ResultMapWithBLOBs`，用于将数据库查询结果映射到实体类 `BBSPost` 的属性。

- **SQL 片段（SQL Fragments）**：定义了两个 SQL 片段，`Base_Column_List` 和 `Blob_Column_List`，用于在查询语句中引用基本列和长文本列。

- **查询操作（Select Statements）**：包括根据帖子ID查询帖子详细信息、根据一组帖子ID查询帖子列表、获取近一周内热门帖子列表、根据条件查询帖子列表和获取特定用户的帖子列表等。

- **插入、更新和删除操作（Insert, Update, Delete Statements）**：定义了插入帖子、更新帖子信息和删除帖子等数据库操作。

BBSUserMapper.xml

 XML 文件是用于 MyBatis 的数据库映射配置，定义了与用户管理相关的数据库操作，主要包括：

- **结果映射（ResultMap）**：定义了一个结果映射 `BaseResultMap`，用于将数据库查询结果映射到实体类 `BBSUser` 的属性。
- **SQL 片段（SQL Fragments）**：定义了一个 SQL 片段 `Base_Column_List`，用于在查询语句中引用基本列。
- **查询操作（Select Statements）**：包括根据用户ID查询用户详细信息、根据一组用户ID查询用户列表、根据登录名查询用户信息以及根据登录名和密码查询用户信息等。
- **插入、更新和删除操作（Insert, Update, Delete Statements）**：定义了插入用户、更新用户信息和删除用户等数据库操作。
- 联系我：![](https://skymirror-1322372781.cos.ap-beijing.myqcloud.com/微信图片_20240131170643.jpg)
