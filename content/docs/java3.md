---
title: Javase 进阶
categories:
  - javase
  
abbrlink: 2701
date: 2024.8.4
tags: 
   - java

---

# java 进阶

## 单元测试

针对最小的功能单元（方法）编写代码对其进行正确性测试

junit单元测试框架

- 灵活的编写测试代码，可以单个测试，也可以一键完成

- 还会生成测试报告

步骤：

- 为需要测试的业务类，定义编写对应的测试类。（公共无参数无返回值）
- 测试方法必须声明@Test注解

```
package com.example.aaa;

import org.junit.Test;
import static org.junit.Assert.*;

public class test1 {

    @Test
    public void testtest1() {
        // 这里是测试逻辑，例如断言
        assertTrue(true);
    }
}

```

例如

### 断言

最核心的，预测的一种机制

```
assertEquals('方法内部有BUG', 4, 5);

```

- `assertEquals` 是 JUnit 中的一个静态方法，用于验证两个值是否相等。

- `'方法内部有BUG'` 是一个字符串，这是在断言失败时会显示的错误消息。

- `4` 是期望值 (expected value)。

- `5` 是实际值 (actual value)。

### 注解

- @Test  
- @Before 修饰一个实例方法，在每一个测试方法执行前执行一次
- @After 修饰一个实例方法，在每一个测试方法执行后执行一次
- @BeforeClass 修饰一个静态方法，在所有测试方法之前执行一次
- @AfterClass 修饰一个静态方法，在所有测试方法之后执行一次

在5.0以后的版本后

@Before变成了BeforeEach

@BeforeClass变成了BeforeAll

例子：

```java
package java go1;

public class StringUtils {

    // 反转字符串
    public String reverseString(String input) {
        if (input == null) {
            return null;
        }
        return new StringBuilder(input).reverse().toString();
    }
}

```

```java
package javago;
import org.junit.Test;
import static org.junit.Assert.*;

public class StringUtilsTest {

    @Test
    public void testReverseString() {
        StringUtils utils = new StringUtils();

        // 测试空字符串
        assertEquals("", utils.reverseString(""));

        // 测试一般字符串
        assertEquals("dcba", utils.reverseString("abcd"));

        // 测试单个字符
        assertEquals("a", utils.reverseString("a"));

        // 测试 null
        assertNull(utils.reverseString(null));
    }
}

```

然后我发现测试通过了

```
C:\Users\ikeife\.jdks\corretto-21.0.3\bin\java.exe -ea -Didea.test.cyclic.buffer.size=1048576 "-javaagent:D:\IntelliJ IDEA 2023.3.7\lib\idea_rt.jar=52251:D:\IntelliJ IDEA 2023.3.7\bin" -Dfile.encoding=UTF-8 -Dsun.stdout.encoding=UTF-8 -Dsun.stderr.encoding=UTF-8 -classpath "D:\IntelliJ IDEA 2023.3.7\lib\idea_rt.jar;D:\IntelliJ IDEA 2023.3.7\plugins\junit\lib\junit5-rt.jar;D:\IntelliJ IDEA 2023.3.7\plugins\junit\lib\junit-rt.jar;E:\work-testroot\study\java\javago\out\production\java go1;C:\Users\ikeife\.m2\repository\org\jetbrains\kotlin\kotlin-stdlib-jdk8\1.9.23\kotlin-stdlib-jdk8-1.9.23.jar;C:\Users\ikeife\.m2\repository\org\jetbrains\kotlin\kotlin-stdlib\1.9.23\kotlin-stdlib-1.9.23.jar;C:\Users\ikeife\.m2\repository\org\jetbrains\annotations\13.0\annotations-13.0.jar;C:\Users\ikeife\.m2\repository\org\jetbrains\kotlin\kotlin-stdlib-jdk7\1.9.23\kotlin-stdlib-jdk7-1.9.23.jar;C:\Users\ikeife\.m2\repository\junit\junit\4.13.1\junit-4.13.1.jar;C:\Users\ikeife\.m2\repository\org\hamcrest\hamcrest-core\1.3\hamcrest-core-1.3.jar" com.intellij.rt.junit.JUnitStarter -ideVersion5 -junit4 javago.StringUtilsTest

进程已结束，退出代码为 0
```

## 反射

加载类，并允许以编程的方式解剖类中的各种成分（成员变量。方法，构造器）

如何获取类的信息并操作他们

- 加载类，获取字节码：获取了class对象
- 获取类的构造器 Constructor对象
- 获取类的成员变量 Field对象
- 获取类的成员方法 Method对象

应用场景：

获取Class对象

- Class c1 = 类名.class

- 调用Class提供方法：public static Class forName(String package);

- Object提供的方法：public Class getClass(); Class c3 = 对象.getClass();

  Test1Class.java(三种方法都有了)

```
package javahight;

public class Test1Class{
    public static void main(String[] arges){
        Class c1 = Students.class;
        System.out.println(c1.getName());//运行可得javahight.Students是包名+类名 叫做全类名
        System.out.println(c1.getSimpleName());//得到类名 Students
        
        
        Class c2 = Class.forName("javahight.Students");
        System.out.println(c1==c2);
        
        
        Students s = new Students();
        Class c3 = s.getClass();
    }
}
```

Students.java

```
package javahight;
class Students{

}

```

**获取类完成了**

#### 获取类的构造器

方法：

```
package javahight;

import java.lang.reflect.Constructor;

class Test1 {
    public static void main(String[] args) {
        testGetConstructor();
    }

    public static void testGetConstructor() {
        Class<?> c = Students.class;//获取类
        Constructor<?>[] constructors = c.getConstructors(); // 获取类的全部构造器
        for (Constructor<?> constructor : constructors) {//遍历所以
            System.out.println(constructor.getName() + " has " + constructor.getParameterCount() + " parameters");//输出
        }
    }
}

```

这个方法只能拿pubilc的

```java
public static void testGetConstructor() {
        Class<?> c = Students.class;//获取类
        Constructor<?>[] constructors = c.getDeclaredConstructors(); // 获取类的全部构造器
        for (Constructor<?> constructor : constructors) {//遍历所以
            System.out.println(constructor.getName() + " has " + constructor.getParameterCount() + " parameters");//输出
        }
    }
```

**改成getDeclaredConstructors()就能拿到所有的**

**这个强烈推荐**

```java
 Class c = Cat.class;
 Constructor constructor = c.getConstructor();
```

这个也是只能拿pubilc的构造器

所以

```java
Class c = Cat.class;
Constructor constructor = c.getDeclaredConstructor();
```

这样就能拿到除public之外的了

要获取的有参数

例如：

```java
public  Students(String name, int age) {
     this.name = name;
     this.age = age;
}
```

方法是有参数的

```java
Constructor constructor = c.getDeclaredConstructor(String.class,int.class);
```

这样就能拿到有参数的构造器了

```java
constructor.newInstance;
```

这个是有权限问题的

所以我们要加上暴力反射，禁止检查访问权限

```java
public static void testGetConstructo() throws Exception{
        Class s = Students.class;
        Constructor constructor = s.getDeclaredConstructor();
        constructor.setAccessible(true);//权限的禁用
        Students students = (Students) constructor.newInstance();
        System.out.println(students);

    }
```

这样的话我们就能拿到无参的构造器

return

```
Students{name='null', age=0}
```

如果用到有参数的，在参数的位置加上参数类型

```java
public static void testGetConstructo() throws Exception{
        Class s = Students.class;
        Constructor constructor = s.getDeclaredConstructor();
        constructor.setAccessible(true);
        Students students = (Students) constructor.newInstance();
        System.out.println(students);
        Constructor constructor2 = s.getDeclaredConstructor(String.class,int.class);
        Students students2  = (Students) constructor2.newInstance("name",6);
        System.out.println(students2);
    }
```

return

```java
Students{name='null', age=0}
Students{name='name', age=6}
```

**注：**

如何没重写toString方法的话，那样只能回显一个内存的地址

所以要在Students加上toString方法 

**重写 `toString` 方法**:

- 在 `Students` 类中添加 `toString` 方法，使其返回包含对象属性的字符串。这样可以确保在打印 `Students` 对象时，会显示对象的属性而不是默认的类名和内存地址。

```java
 @Override
    public String toString() {
        return "Students{name='" + name + "', age=" + age + "}";
    }
```

#### **获取类的成员变量**

```
public static class Test {
        public void testGetFields() {
            Class<?> s = Students.class;//反射
            Field[] fields = s.getDeclaredFields(); // 获取类的所有字段
            for (Field field : fields) {//变量输出
                System.out.println(field.getName() + " : " + field.getType());
            }
        }
    }
```

return

```
name : class java.lang.String
age : int
a : int
country : class java.lang.String

```

拿之前的方法就也能拿到单个的成员变量

```
 Field fname = s.getDeclaredField("name");
 System.out.println(fname.getName());
```

return：

```
name
```

接下来是为了成员变量赋值

```
 try {
                Field fname = s.getDeclaredField("name");
                System.out.println(fname.getName());

                Students students = new Students();
                fname.setAccessible(true);
                fname.set(students, "大"); // 设置students对象的name字段
                System.out.println(students); // 打印students对象，查看修改后的name字段
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
```

最后回显

```
name
Students{name='大', age=0}
```

#### 获取成员方法

```
public void testGetFields() {
            Class s = Students.class;//反射第一步
             Method[] methods = s.getDeclaredMethods();//获取方法
             for(Method method : methods){//遍历输出
                 System.out.println(method.getName() + "---" + method.getParameterCount() + "---" + method.getReturnType());
            }
        }
```

回显

```
getA---0---int
setAge---1---void
getAge---0---int
setA---1---void
printStudentInfo---0---void
getName---0---class java.lang.String
toString---0---class java.lang.String
setName---1---void
getCountry---0---class java.lang.String
```

拿取单个方法，定位需要方法名 方法的参数类型来定位

```
Class s = Students.class;
            String setAge;
            Method method = s.getDeclaredMethod("getName");
             System.out.println(method.getName() + "---" + method.getParameterCount() + "---" + method.getReturnType());
```

回显

```
getName---0---class java.lang.String
```

```
static class Test {
        public void testInvokeMethod() throws Exception {
            Class<?> s = Students.class;
            Method method = s.getDeclaredMethod("printStudentInfo");

            Students students = new Students("Alice", 20);
            method.setAccessible(true);
            method.invoke(students);

            System.out.println("aaa");//触发执行
        }
    }
```

return

```
Name: Alice, Age: 20, Country: china
aaa
```

#### 反射的作用

适合做java的框架

简易框架的开发：

Test1Class.java

```
package javahight;

import org.junit.Test;

import java.io.ObjectStreamException;

public class Test1Class{
    @Test
    public void save() throws Exception {
        Students s1 = new Students("zhou","18");
        ObjectSaver.saveObject(s1);
    }
}
```

ObjectSaver.java

```
package javahight;

import java.io.File;
import java.io.FileOutputStream;
import java.io.PrintStream;
import java.lang.reflect.Field;

public class ObjectSaver { // 修改类名避免与 java.lang.Object 冲突
    public static void saveObject(Object obj) throws Exception {
        // 设置文件路径，确保路径有效
        PrintStream ps = new PrintStream(new FileOutputStream("output.txt", true));
        Class c = obj.getClass();//反射第一步
        Field[] fields = c.getDeclaredFields();//获取成员变量
        String cName = c.getSimpleName();//获取简短名称
        ps.println("------" + cName + "-------");//输出-----Students----
        for (Field field : fields) {
            String name = field.getName();//获取名字
            field.setAccessible(true); // 修正为 field.setAccessible(true)//遍历成员变量
            String value = field.get(obj) + "";//获取值
            ps.println(name + "=" + value);//输出
        }
        ps.close(); // 关闭 PrintStream
    }
}
```

Students.java

```
package javahight;

class Students {
    private String name;
    private int age;

    public Students(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Students{name='" + name + "', age=" + age + "}";
    }
}

```

output.txt

```
------Students-------
name=zhou
age=18
```

## 注解

java代码里的特殊标记，让其他程序根据注解信息来怎么执行程序

可以用在类/构造器/方法/成员变量。。。

定义注解:

```
public @interface 注解名称{
public 属性类型 属性名() default 默认值；
}
```

```
@anntation(aaa="aaa",ccc="java")
public class test {
    @anntation(aaa="aaaaaaa",ccc="ddddddd")
    public void test2(){
        
    }
}
```

```
public @interface anntation {
    String aaa();
    boolean bbb() default true;
    String [] ccc();
}

```

例如：

**特殊情况**

如果注解只有一个值而且为value value可以省略

注解的原理：

注解的本质是

- 是一个接口，继承自Annotation
- 使用@注解的适合，就是一个实现类对象，实现了注解以及Annotation

#### 元注解

**修饰注解的注解**

```

@Target(ElementType.TYPE)//声明被修饰的注解只能在哪使用
@Retention(RetentionPolicy.CLASS)//注解的保留周期
public @interface anntation {
    String aaa();
    boolean bbb() default true;
    String [] ccc();
}

```

上面那两个就是元注解

Target（ElementType.TYPE）注解：

- Type 类 接口
- FIELD 成员变量
- METHOD 成员方法
- PARAMETER 方法参数
- CONSTRUCTOR 构造器
- LOCAL_VARIBLE 局部变量

Retention()

- SOURCE 源码阶段
- CLASS 字节码文件
- **RUNTIME 运行的阶段**  运用最多的

#### 注解的解析

要解析谁上的注解，就应该先拿到谁

```
public class testannotation {
    @Test
    public void parseClass(){
        Class c = test.class;
        //判断类上是不是包含了注解
        if(c.isAnnotationPresent(anntation.class)) {
            (anntation) c.getDeclaredAnnotations(anntation.class);
            System.out.println();
    }
}
}
```

解析类的对象

#### 应用场景

配合反射做框架的

MyTest.java

```
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface MyTest {

}
```

Test1.java

```
import java.lang.reflect.Method;

public class Test1 {

    public static class Test {
        @MyTest
        public void test1() {
            System.out.println("aaaaaa");
        }
    }

    public static void main(String[] args) {
        try {
            Test a = new Test();
            Class<?> c = Test.class;
            Method[] methods = c.getDeclaredMethods();
            for (Method method : methods) {
                if (method.isAnnotationPresent(MyTest.class)) {
                    method.invoke(a);
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```

return:

```
aaaaaa
```

## 动态代理

程序为什么要代理？

使用接口进行代理

建立了一个初级的代理：

BigStar.java

```
public class BigStar implements Star {
    private String name;//java约定俗称的
    public BigStar(String name){
        this.name = name;
    }
    public void sing(String name){
        System.out.println(this.name + "正在唱"+ name);
    }
    public void dance(){
        System.out.println(this.name+"正在跳舞");
    }
}
```

Star.java

```
public interface Star {
    void sing(String name);
    void dance();//两个方法都
}

```

**将对象的东西转移到代理上，这是个初级的运用**



