---
title: javase 特殊文件&&日志
categories:
  - javase
abbrlink: 2701
date: 2024.8.20
tags: 
   - java

---

# 简述

properties文件

是一个属性文件，以键值对形式，不能重复。可以存储用户名和密码

xml文件用于数据存储和关系，存多个用户的时候，可以使用xml文件来存储用户名的多个信息

为系统配置作为信息

日志是把程序运行的信息记录到文件中去

# properties

使用Map集合中的properties

不要把他当集合用，用来读写properties属性文件

```
import java.io.*;
import java.util.Properties;

public class Test {
    public static void main(String[] args) throws Exception {
        Properties properties = new Properties();
        properties.load(new FileReader("users.properties"));
        System.out.println(properties);

    }
}
```

properties的一系列方法

```
import java.io.*;
import java.util.Properties;
import java.util.Set;

public class Test {
    public static void main(String[] args) throws Exception {
        Properties properties = new Properties();
        properties.load(new FileReader("users.properties"));
        System.out.println(properties);
        System.out.println(properties.getProperty("admin"));//获得这个key的value
        Set<String> keys = properties.stringPropertyNames();
        for (String key : keys){
            String value = properties.getProperty(key);
            System.out.println(key + value);
        }//增强for循环遍历键值对
        properties.forEach((k,v)->{
            System.out.println(k+"---"+v);
        });//表达式遍历
        properties.setProperty("admin2","123456799");
        properties.store(new FileWriter("users.properties"),"qwq");//向文件中添加属性


    }
}
```

# xml

可扩展标记语言，这些标签是可以自己定义的

本质上是一种特殊的格式，可以用来存储复杂的数据结构，和数据关系

跟html类似，可以定义，扩展等

文件的后缀一般是.xml

```
<?xml version="1.0" encoding="utf-8" ?>
<!-- 这是备注-->
<users>
    <name>aaaa</name>
    <login>qwqwq</login>
    <password>123456</password>
</users>

```

在xml文件中并不是什么都能写的

<>都会出现冲突，因为会认为是标签

可以用如下字符替代

- &lt 小于
- &gt 大于
- &amp 和
- &apos 单引号
- &quot 引号

```
<![CDATA[
AAAAAAFDFDFDFDFDFDF
]]>
```

在这个数据区里面可以随便写任何的东西，不会有问题

xml文件可以在网络中进行传输

## 读取xml文件中的数据

解析xml的框架

**Dom4j**这是最好用的

用SAXReader解析器解析，然后根据元素来获取

```
import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

public class Test {
    public static void main(String[] args) throws Exception {
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read("aaa.xml");
        Element element = document.getRootElement();
        System.out.println(element);
    }
    }
```

简单的获取根元素对象

是一种自上而下的解析方式，从父到子一级一级的解析

```
import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import java.util.List;

public class Test {
    public static void main(String[] args) throws Exception {
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read("aaa.xml");
        Element root = document.getRootElement();

        List<Element> elements =  root.elements();
        for (Element element:elements){
            System.out.println(element.getName());
        }//遍历输出
        Element name = root.element("name");//默认获取name
        System.out.println(name);
        System.out.println(users.elementText(name));//获取文本内容
        
    }
    }
```



# 日志

用输出语句展示日志，会显示在控制台。这个方法是不靠谱的，而且也不方便。取消日志的话还要修改源代码

​	日志技术可以将系统执行的信息，方便的写道指定的位置

​	随时可以用开关关闭开始

可是使用日志框架

- jul
- log4j
- **logback**
- 其他的等

使用日志框架的一套标准日志接口

- JCl
- **slf4j**

加粗的是用的最多的一个

## logback

分为三个模块

- 核心core模块
- 登录经典
- 日志回访

**slf4j+core+logback-access**

使用maven工具直接进行加载，将jar包导入到项目中去

然后将lockback.xml放到src目录下面就行（直接拷贝过来就行）

创建logback的logger对象的getlogger方法来获取日志

其中的Info表示重要的信息（里面是提示）

error表示错误的日志

debug表示调试，即是程序的处理

logback.xml里面的配置

通常可以设置两个输出位置

- 控制台
- txt文件

设置日志级别

- trace 
- debug 一般作为最低级别 最常用
- info 重要，数据连接网络连接io操作等
- warn 警告
- error 错误信息

**合理的输出信息**

**在发开中为了方便定位信息，只想记录某些特别的地方**

进行设置日志级别**只有大于等于这个的才会显示**

