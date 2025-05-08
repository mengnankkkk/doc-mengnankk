---
title: Spring boot(1)
categories:
  - 技术栈
  
abbrlink: 2701
date: 2024.6.25
tags: 
   - java 
   - spring boot

---

# Spring boot

可构建的系统：Maven |Gradle|Ant|Starters

运行代码： IDE｜ Packaged｜ Maven｜ Gradle  

你可以使用Spring Boot创建Java应用， 并使用 java -jar 启动它或采用传统的
war部署方式。  

Spring Boot提供了一个可选的Maven插件， 用于创
建可执行jars。  下面是一个典型的pom.xml文件：  

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="ht
tp://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http:/
/maven.apache.org/xsd/maven-4.0.0.xsd">
<modelVersion>4.0.0</modelVersion>
<groupId>com.example</groupId>
<artifactId>myproject</artifactId>
<version>0.0.1-SNAPSHOT</version>
<!-- Inherit defaults from Spring Boot -->
<parent>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-parent</artifactId>
<version>2.0.0.RELEASE</version>
</parent>
<!-- Add typical dependencies for a web application -->
<dependencies>
<dependency>
35<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
</dependency>
</dependencies>
<!-- Package as an executable jar -->
<build>
<plugins>
<plugin>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
</plugins>
</build>
</project>
```

Gradle Wrapper提供一种给力的获取Gradle的方式。 它是
一小段脚本和库， 跟你的代码一块提交， 用于启动构建进程  下面是一个典型的 build.gradle 文件：  

```gradle
plugins {
id 'org.springframework.boot' version '2.0.0.RELEASE'
id 'java'
} j
ar {
baseName = 'myproject'
version = '0.0.1-SNAPSHOT'
} r
epositories {
jcenter()
} d
ependencies {
compile("org.springframework.boot:spring-boot-starter-web")
testCompile("org.springframework.boot:spring-boot-starter-te
st")
}
```

## 创建一个简单的spring boot应用

### 创建pom文件

创建一个Maven pom.xml 文件作为开始  ，打开你最喜欢的文本编辑器， 并添加以下内容：  

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="ht
tp://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http:/
/maven.apache.org/xsd/maven-4.0.0.xsd">
<modelVersion>4.0.0</modelVersion>
<groupId>com.example</groupId>
<artifactId>myproject</artifactId>
<version>0.0.1-SNAPSHOT</version>
<parent>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-parent</artifactId>
<version>2.0.0.RELEASE</version>
</parent>
<!-- Additional lines to be added here... -->
</project>
```

### 简单的java文件

```java
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.*;
import org.springframework.web.bind.annotation.*;
@RestController
@EnableAutoConfiguration
public class Example {
@RequestMapping("/")
String home() {
return "Hello World!";
} p
ublic static void main(String[] args) throws Exception {
SpringApplication.run(Example.class, args);
}
}
```

#### 注解  

Example类上使用的第一个注解是 @RestController ， 这被称为构造型
（ stereotype） 注解。 它为阅读代码的人提供暗示（ 这是一个支持REST的控制
器） ， 对于Spring， 该类扮演了一个特殊角色。 在本示例中， 我们的类是一个web
@Controller ， 所以当web请求进来时， Spring会考虑是否使用它来处理。
@RequestMapping 注解提供路由信息， 它告诉Spring任何来自/路径的HTTP请求
都应该被映射到 home 方法。 @RestController 注解告诉Spring以字符串的形式
渲染结果， 并直接返回给调用者。  

第二个类级别的注解是 @EnableAutoConfiguration ， 这个注解告诉Spring Boot
根据添加的jar依赖猜测你想如何配置Spring。 由于 spring-boot-starter-web 添
加了Tomcat和Spring MVC， 所以auto-configuration假定你正在开发一个web应
用， 并对Spring进行相应地设置。  

#### main方法  

应用程序的最后部分是main方法， 这是一个标准的方法， 它遵循Java对于一个应用
程序入口点的约定。 我们的main方法通过调用 run ， 将业务委托给了Spring Boot
的SpringApplication类。 SpringApplication将引导我们的应用， 启动Spring， 相应
地启动被自动配置的Tomcat web服务器。 我们需要将 Example.class 作为参数传
递给 run 方法， 以此告诉SpringApplication谁是主要的Spring组件， 并传递args数
组以暴露所有的命令行参数。  

#### 创建可执行的jar包

为了创建可执行的jar， 我们需要将 spring-boot-maven-plugin 添加
到 pom.xml 中， 在dependencies节点后面插入以下内容：  

然后mvn package进行打包

#### 继承  

如果你想配置项目， 让其继承自 spring-boot-starter-parent ， 只需
将 parent 按如下设置：  

```xml
<!-- Inherit defaults from Spring Boot -->
<parent>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-parent</artifactId>
<version>2.0.0.RELEASE</version>
</parent>
```

如果你不想使用 spring-boot-starter-parent ， 通过设置 scope=import 的依
赖， 你仍能获取到依赖管理的好处：  

```xml
<dependencyManagement>
<dependencies>
<dependency>
<!-- Import dependency management from Spring Boot -
->
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-dependencies</artifactId>
<version>2.0.0.RELEASE</version>
<type>pom</type>
<scope>import</scope>
</dependency>
</dependencies>
</dependencyManagement>
```

#### 使用Spring Boot Maven插件  

Spring Boot包含一个Maven插件， 它可以将项目打包成一个可执行jar。 如果想使用
它， 你可以将该插件添加到 <plugins> 节点处：  

```xml
<build>
<plugins>
<plugin>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
</plugins>
</build>
```

#### Starters  

Starters是一个依赖描述符的集合， 你可以将它包含进项目中， 这样添加依赖就非
常方便。 你可以获取所有Spring及相关技术的一站式服务， 而不需要翻阅示例代
码， 拷贝粘贴大量的依赖描述符。 例如， 如果你想使用Spring和JPA进行数据库访
问， 需要在项目中包含 spring-boot-starter-data-jpa 依赖。  

表 13.1. Spring Boot application starters

| 名称                         | 描述                                         | Pom  |
| ---------------------------- | -------------------------------------------- | ---- |
| spring-boot-starter          | 核心starter， 包括自动配置支持， 日志和YAML  | Pom  |
| spring-boot-starter activemq | 用于使用Apache ActiveMQ实现JMS消息           | Pom  |
| spring-boot-starter amqp     | 用于使用Spring AMQP和Rabbit MQ               | Pom  |
| spring-boot-starter aop      | 用于使用Spring AOP和AspectJ实现面向切面 编程 | Pom  |
| spring-boot-starter artemis  | 使用Apache Artemis实现JMS消息                | Pom  |
| spring-boot-starter batch    | 对Spring Batch的支持                         | Pom  |

13.5. Starters
71

| spring-boot-starter cache                   | 用于使用Spring框架的缓存支持                                 | Pom  |
| ------------------------------------------- | ------------------------------------------------------------ | ---- |
| spring-boot-starter cloud-connectors        | 对Spring Cloud Connectors的支持， 用于简化 云平台下（ 例如Cloud Foundry 和Heroku） 服 务的连接 | Pom  |
| spring-boot-starter data-cassandra          | 用于使用分布式数据库Cassandra和Spring Data Cassandra         | Pom  |
| spring-boot-starter data-cassandra reactive | 用于使用分布式数据库Cassandra和Spring Data Cassandra Reactive | Pom  |
| spring-boot-starter data-couchbase          | 用于使用基于文档的数据库Couchbase和 Spring Data Couchbase    | Pom  |
| spring-boot-starter data-couchbase reactive | 用于使用基于文档的数据库Couchbase和 Spring Data Couchbase Reactive | Pom  |
| spring-boot-starter data-elasticsearch      | 用于使用Elasticsearch搜索， 分析引擎和 Spring Data Elasticsearch | Pom  |
| spring-boot-starter data-jpa                | 用于使用Hibernate实现Spring Data JPA                         | Pom  |
| spring-boot-starter data-ldap               | 用于Spring Data LDAP                                         | Pom  |
| spring-boot-starter data-mongodb            | 用于使用基于文档的数据库MongoDB和Spring Data MongoDB         | Pom  |
| spring-boot-starter data-mongodb reactive   | 用于使用基于文档的数据库MongoDB和Spring Data MongoDB Reactive | Pom  |
| spring-boot-starter data-neo4j              | 用于使用图数据库Neo4j和Spring Data Neo4j                     | Pom  |
| spring-boot-starter data-redis              | 用于使用Spring Data Redis和Lettuce客户端操 作键-值存储的Redis | Pom  |
| spring-boot-starter data-redis-reactive     | 用于使用Spring Data Redis reactive和Lettuce 客户端操作键-值存储的Redis | Pom  |
| spring-boot-starter data-rest               | 用于使用Spring Data REST暴露基于REST的 Spring Data仓库       | Pom  |
| spring-boot-starter data-solr               | 通过Spring Data Solr使用Apache Solr搜索平 台                 | Pom  |
| spring-boot-starter freemarker              | 用于使用FreeMarker模板引擎构建MVC web应 用                   | Pom  |

13.5. Starters
72

| spring-boot-starter groovy-templates  | 用于使用Groovy模板引擎构建MVC web应用                        | Pom  |
| ------------------------------------- | ------------------------------------------------------------ | ---- |
| spring-boot-starter hateoas           | 用于使用Spring MVC和Spring HATEOAS实现 基于超媒体的RESTful web应用 | Pom  |
| spring-boot-starter integration       | 用于使用Spring Integration                                   | Pom  |
| spring-boot-starter jdbc              | 对JDBC的支持（ 使用Tomcat JDBC连接池）                       | Pom  |
| spring-boot-starter jersey            | 用于使用JAX-RS和Jersey构建RESTful web应 用， 可使用spring-boot-starter-web替代 | Pom  |
| spring-boot-starter jooq              | 用于使用JOOQ访问SQL数据库， 可使 用spring-boot-starter-data-jpa或spring-boot starter-jdbc替代 | Pom  |
| spring-boot-starter json              | 用于读写json                                                 | Pom  |
| spring-boot-starter jta-atomikos      | 用于使用Atomikos实现JTA事务                                  | Pom  |
| spring-boot-starter jta-bitronix      | 用于使用Bitronix实现JTA事务                                  | Pom  |
| spring-boot-starter jta-narayana      | Spring Boot Narayana JTA Starter                             | Pom  |
| spring-boot-starter mail              | 用于使用Java Mail和Spring框架email发送支持                   | Pom  |
| spring-boot-starter mustache          | 用于使用Mustache模板引擎构建MVC web应用                      | Pom  |
| spring-boot-starter quartz            | Spring Boot Quartz Starter                                   | Pom  |
| spring-boot-starter security          | 对Spring Security的支持                                      | Pom  |
| spring-boot-starter security-reactive | 对响应式Spring Security的支持                                | Pom  |
| spring-boot-starter test              | 用于测试Spring Boot应用， 支持常用测试类 库， 包括JUnit, Hamcrest和Mockito | Pom  |
| spring-boot-starter thymeleaf         | 用于使用Thymeleaf模板引擎构建MVC web应 用                    | Pom  |
| spring-boot-starter validation        | 用于使用Hibernate Validator实现Java Bean校 验                | Pom  |

13.5. Starters
73

| spring-boot-starter web          | 用于使用Spring MVC构建web应用， 包括 RESTful。 Tomcat是默认的内嵌容器 | Pom  |
| -------------------------------- | ------------------------------------------------------------ | ---- |
| spring-boot-starter web-services | 对Spring Web服务的支持                                       | Pom  |
| spring-boot-starter webflux      | 提供用Spring框架创建webflux应用的支持                        | Pom  |
| spring-boot-starter websocket    | 用于使用Spring框架的WebSocket支持构建 WebSocket应用          | Pom  |

除了应用程序starters， 以下starters可用于添加production ready的功能：
表 13.2. Spring Boot生产级starters

| 名称                         | 描述                                                         | Pom  |
| ---------------------------- | ------------------------------------------------------------ | ---- |
| spring-boot starter actuator | 用于使用Spring Boot的Actuator， 它提供了 production ready功能来帮助你监控和管理应用程序 | Pom  |

最后， Spring Boot还包含以下用于排除或交换某些特定技术方面的starters：
表 13.3. Spring Boot技术性starters

| 名称                              | 描述                                                         | Pom  |
| --------------------------------- | ------------------------------------------------------------ | ---- |
| spring-boot starter-jetty         | 用于使用Jetty作为内嵌servlet容器， 可使 用spring-boot-starter-tomcat替代 | Pom  |
| spring-boot starter-log4j2        | 用于使用Log4j2记录日志， 可使用spring-boot starter-logging代替 | Pom  |
| spring-boot starter-logging       | 用于使用Logback记录日志， 默认的日志starter                  | Pom  |
| spring-boot starter-reactor netty | 使用Reactor Netty做为内嵌的HTTP服务器                        | Pom  |
| spring-boot starter-tomcat        | 用于使用Tomcat作为内嵌servlet容器， spring boot-starter-web使用的默认servlet容器 | Pom  |
| spring-boot starter-undertow      | 用于使用Undertow作为内嵌servlet容器， 可使 用spring-boot-starter-tomcat替代 | Pom  |

注： 查看GitHub上位于 spring-boot-starters 模块内的README文件， 可以获
取到一个社区贡献的其他starters列表。

### 组织你的代码

#### default包

当类没有声明 package 时， 它被认为处于 default package 下。 通常不推荐使
用 default package ， 因为对于使
用 @ComponentScan ， @EntityScan 或 @SpringBootApplication 注解的
Spring Boot应用来说， 它会扫描每个jar中的类， 这会造成一定的问题。
注 我们建议你遵循Java推荐的包命名规范， 使用一个反转的域名（ 例
如 com.example.project ） 。  