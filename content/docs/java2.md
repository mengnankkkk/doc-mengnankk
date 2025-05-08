---
title: Javase学习基础（2）
categories:
  - javase
  
abbrlink: 2701
date: 2024.4.23
tags: 
   - java

---

# java中级

## 异常处理

导致程序的正常流程被中断的事件，叫做异常

常见手段： try catch  finally  throws

###  try catch

将可能抛出FileNotFoundException **文件不存在异常**的代码放在try里
2.如果文件存在，就会顺序往下执行，并且不执行catch块中的代码
\3. 如果文件不存在，try 里的代码会立即终止，程序流程会运行到对应的catch块中
\4. e.printStackTrace(); 会打印出方法的调用痕迹，如此例，会打印出异常开始于TestException的第16行，这样就便于定位和分析到底哪里出了异常

```java
package exception;
 
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
 
public class TestException {
 
    public static void main(String[] args) {
         
        File f= new File("d:/LOL.exe");
         
        try{
            System.out.println("试图打开 d:/LOL.exe");
            new FileInputStream(f);
            System.out.println("成功打开");
        }
        catch(FileNotFoundException e){
            System.out.println("d:/LOL.exe不存在");
            e.printStackTrace();
        }
         
    }
}
```

使用异常的父类进行catch也可以

多种异常可以分别解决

```java
package exception;
 
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
 
public class TestException {
 
    public static void main(String[] args) {
 
        File f = new File("d:/LOL.exe");
 
        try {
            System.out.println("试图打开 d:/LOL.exe");
            new FileInputStream(f);
            System.out.println("成功打开");
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
            Date d = sdf.parse("2016-06-03");
        } catch (FileNotFoundException e) {
            System.out.println("d:/LOL.exe不存在");
            e.printStackTrace();
        } catch (ParseException e) {
            System.out.println("日期格式解析错误");
            e.printStackTrace();
        }
    }
}
```

也可以多个异常统一catch解决，但是不能知道是哪一个的问题

```java
package exception;
 
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
 
public class TestException {
 
    public static void main(String[] args) {
 
        File f = new File("d:/LOL.exe");
 
        try {
            System.out.println("试图打开 d:/LOL.exe");
            new FileInputStream(f);
            System.out.println("成功打开");
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
            Date d = sdf.parse("2016-06-03");
        } catch (FileNotFoundException | ParseException e) {
            if (e instanceof FileNotFoundException)
                System.out.println("d:/LOL.exe不存在");
            if (e instanceof ParseException)
                System.out.println("日期格式解析错误");
 
            e.printStackTrace();
        }
 
    }
}
```

### finally

无论是否出现异常，finally中的代码都会被执行

```java
package exception;
 
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
 
public class TestException {
 
    public static void main(String[] args) {
         
        File f= new File("d:/LOL.exe");
         
        try{
            System.out.println("试图打开 d:/LOL.exe");
            new FileInputStream(f);
            System.out.println("成功打开");
        }
        catch(FileNotFoundException e){
            System.out.println("d:/LOL.exe不存在");
            e.printStackTrace();
        }
        finally{
            System.out.println("无论文件是否存在， 都会执行的代码");
        }
    }
}
```

### throws方法

考虑如下情况：
主方法调用method1
method1调用method2
method2中打开文件

method2中需要进行异常处理
但是method2**不打算处理**，而是把这个异常通过**throws****抛出去**
那么method1就会**接到该异常**。 处理办法也是两种，要么是try catch处理掉，要么也是**抛出去**。
method1选择本地try catch住 一旦try catch住了，就相当于把这个异常消化掉了，主方法在调用method1的时候，就不需要进行异常处理了

```java
package exception;
 
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
 
public class TestException {
 
    public static void main(String[] args) {
        method1();
 
    }
 
    private static void method1() {
        try {
            method2();
        } catch (FileNotFoundException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
 
    }
 
    private static void method2() throws FileNotFoundException {
 
        File f = new File("d:/LOL.exe");
 
        System.out.println("试图打开 d:/LOL.exe");
        new FileInputStream(f);
        System.out.println("成功打开");
 
    }
}
```

throws与throw这两个关键字接近，不过意义不一样，有如下区别：
\1. throws 出现在方法声明上，而throw通常都出现在方法体内。
\2. throws 表示出现异常的一种可能性，并不一定会发生这些异常；throw则是抛出了异常，执行throw则一定抛出了某个异常对象。

## 异常分类

 可查异常，运行时异常和错误3种
其中，运行时异常和错误又叫非可查异常

### 可查异常

CheckedException
可查异常即**必须进行处理的异常**，要么try catch住,要么往外抛，谁调用，谁处理，比如 FileNotFoundException
如果不处理，编译器，就不让你通过

### 运行时异常

RuntimeException指： **不是必须进行try catch的异常**

**常见运行时异常:**
除数不能为0异常:ArithmeticException
下标越界异常:ArrayIndexOutOfBoundsException
空指针异常:NullPointerException
在编写代码的时候，依然可以使用try catch throws进行处理，与可查异常不同之处在于，**即便不进行try catch，也不会有编译错误**
Java之所以会设计运行时异常的原因之一，是因为下标越界，空指针这些运行时异常**太过于普遍**，如果都需要进行捕捉，代码的可读性就会变得很糟糕。

```java
package exception;
  
public class TestException {
  
    public static void main(String[] args) {
         
        //任何除数不能为0:ArithmeticException
        int k = 5/0;
         
        //下标越界异常：ArrayIndexOutOfBoundsException
        int j[] = new int[5];
        j[10] = 10;
         
        //空指针异常：NullPointerException
        String str = null;
        str.length();
   }
}
```

### 错误

错误Error，指的是**系统级别的异常**，通常是内存用光了
在**默认设置下**，一般java程序启动的时候，最大可以使用16m的内存
如例不停的给StringBuffer追加字符，很快就把内存使用光了。抛出**OutOfMemoryError**
与运行时异常一样，错误也是不要求强制捕捉的

代码比较复制代码

![三种分类](https://stepimagewm.how2j.cn/2412.png)

### Throwable

是类，Exception和Error都继承了该类
所以在捕捉的时候，也可以使用Throwable进行捕捉
如图： 异常分**Error**和**Exception**
Exception里又分**运行时异常**和**可查异常**。

![Throwable](https://stepimagewm.how2j.cn/742.png)

```java
package exception;
 
import java.io.File;
import java.io.FileInputStream;
 
public class TestException {
 
    public static void main(String[] args) {
 
        File f = new File("d:/LOL.exe");
 
        try {
            new FileInputStream(f);
            //使用Throwable进行异常捕捉
        } catch (Throwable t) {
            // TODO Auto-generated catch block
            t.printStackTrace();
        }
 
    }
}
```

应用：

```java
public class TestException  {
    public static void main(String[] args)  {
        method1();
      /*int a=  getresult();
        System.out.println(a);*/
 
    }
    public static void method() throws Throwable {
        File f=new File("d:/lol");
        new FileInputStream(f);
    }
    public static void method1()  {
        try {
            method();
        } catch (Throwable e) {
            e.printStackTrace();
 
        }
    }
    public static int getresult(){
        File f=new File("d:/");
       try {
           new FileInputStream(f);
           return 1;
       }catch (FileNotFoundException e){
           e.printStackTrace();
           return 2;
       }finally {
           return 3;
       }
    }
```

###  自定义异常

1.创建一个类EnemyHeroIsDeadException，并继承Exception
提供两个构造方法
\1. 无参的构造方法
\2. 带参的构造方法，并调用父类的对应的构造方法

```java
class EnemyHeroIsDeadException extends Exception{
     
    public EnemyHeroIsDeadException(){
         
    }
    public EnemyHeroIsDeadException(String msg){
        super(msg);
    }
}
```

 

```java
package charactor;
  
public class Hero {
    public String name;
    protected float hp;
 
    public void attackHero(Hero h) throws EnemyHeroIsDeadException{
        if(h.hp == 0){
            throw new EnemyHeroIsDeadException(h.name + " 已经挂了,不需要施放技能" );
        }
    }
 
    public String toString(){
        return name;
    }
     
    class EnemyHeroIsDeadException extends Exception{
         
        public EnemyHeroIsDeadException(){
             
        }
        public EnemyHeroIsDeadException(String msg){
            super(msg);
        }
    }
      
    public static void main(String[] args) {
         
        Hero garen =  new Hero();
        garen.name = "盖伦";
        garen.hp = 616;
 
        Hero teemo =  new Hero();
        teemo.name = "提莫";
        teemo.hp = 0;
         
        try {
            garen.attackHero(teemo);
             
        } catch (EnemyHeroIsDeadException e) {
            // TODO Auto-generated catch block
            System.out.println("异常的具体原因:"+e.getMessage());
            e.printStackTrace();
        }
         
    }
}
```

## JAVA 的FILE类，以及常用方法

### 创建文件对象

```java
package file;
  
import java.io.File;
  
public class TestFile {
  
    public static void main(String[] args) {
        // 绝对路径
        File f1 = new File("d:/LOLFolder");
        System.out.println("f1的绝对路径：" + f1.getAbsolutePath());
        // 相对路径,相对于工作目录，如果在eclipse中，就是项目目录
        File f2 = new File("LOL.exe");
        System.out.println("f2的绝对路径：" + f2.getAbsolutePath());
  
        // 把f1作为父目录创建文件对象
        File f3 = new File(f1, "LOL.exe");
  
        System.out.println("f3的绝对路径：" + f3.getAbsolutePath());
    }
}
```

