---
title: javase IO流
categories:
  - javase
abbrlink: 2701
date: 2024.8.15
tags: 
   - java
---

# 简介

File代表当前系统下的文件（文件夹）

可以获取文件的信心（大小，文件名，修改时间）

**只能够对文献本身进行操作，不能读写文件里的数据**

**IO流用来读写数据**

**File代表文本，IO流用来代表数据**

# File

## 创建对象

```
import java.io.File;

public class Test {
    public static void main(String[] args) {
      File f1 =  new File("E:\\work-testroot\\study\\java\\javahight\\javahight2\\ab.txt");
        System.out.println(f1.length());
    }
}
```

分割符一般用/

File可以指定一个不存在的文件，其长度为0

分为绝对路径和相对路径

绝对路径是带盘符的

相对路径是模块中的路径，不带盘符

## 常用方法

### 文件信息

```
import java.io.File;

public class Test {
    public static void main(String[] args) {
      File f1 =  new File("ab.txt");
        System.out.println(f1.length());//获取长度
        System.out.printf(String.valueOf(f1.exists()));//是否存在
        System.out.println(f1.isFile());//是否文件
        System.out.println(f1.isDirectory());//是否文件夹
        System.out.println(f1.getName());//获取文件名称带后缀的
        System.out.println(f1.length());
        System.out.println(f1.lastModified());//文件最后修改时间
        System.out.println(f1.getPath());//创建文件时用的路径
        System.out.println(f1.getAbsolutePath());//获取绝对路径
    }
}
```

### 创建删除

```
import java.io.File;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
      File f1 =  new File("ab.txt");
      File f2 = new File("aaaa.txt");
      f2.createNewFile();//创建文件夹
        File f3 = new File("/aaa");
        f3.mkdir();//创建一级文件夹
        f3.mkdirs();//创建多级文件夹
        f1.delete();//删除文件，只能删除空文件夹
    }
}
```

### 遍历文件夹

```
import java.io.File;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
    File f1 = new File("E:\\work-testroot\\study\\java\\javahight\\javahight2");
    String[] names = f1.list();//获取当前目录下所有的一级文件名称
    for (String name: names){
        System.out.println(name);
    }
    File[] files = f1.listFiles();//拿所有一级文件对象到数组中
   for (File file1 : files) {
       System.out.println(file1.length());

        }
    }
}
```

ListFile注意：

- 当主调时文件或者路径不存在会返回null
- 当主掉时空文件夹的时候返回长度为0的数组
- **当主调是一个空文件的时候，将里面一级文件和文件夹的路径放在数组中返回**
- 当主调时一个文件夹里面有一隐藏文件，隐藏文件也会输出
- 没有权限访问文件的时候会返回null

### 案例

```
import java.io.File;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
    File f1 = new File("E:\\work-testroot\\study\\java\\javahight\\javahight2");
    File[] videos = f1.listFiles();
        for (File video : videos) {
            String name =  video.getName();
        String index =  name.substring(name.indexOf("，"));//substring 方法的参数应该是起始和结束的索引。
        String lastname = name.substring(name.indexOf(","));
        String newname = (Integer.valueOf(index+10)+lastname);
        video.renameTo(new File(f1,newname));
        }
    }
}
```

## 方法递归

是一种算法，方法调用自身的形式叫方法递归

- 直接递归，自己调用自己
- 间接递归，方法调用其他方法，其他又调用自己

递归没有处理好终止，会出现死循环，导致内存栈溢出

```
public class Test {
    public static void main(String[] args) {
        System.out.println(f(5));

    }
    public static  int f(int n){
        if(n==1){
            return 1;
        }
        else {
            return f(n-1)*n;
        }
    }
}
```

简单的方法递归

在栈内从栈底开始递归调用一直到终结点，然后pop出去

递归算法三要素

- 递归的公式
- 终结点
- 递归的方式必须走向终结点

### 文件搜索

```
import java.io.File;

public class Test {
    public static void main(String[] args) {
        search(new File("E:/"), "QQ.exe");
    }

    public static void search(File dir, String filename) {
        if (dir == null || !dir.exists()) {//排除目录情况
            return;
        }

        if (dir.isFile()) {//
            if (dir.getName().contains(filename)) {
                System.out.println(dir.getAbsolutePath());//直接能搜索到了
            }
        } else {
            File[] files = dir.listFiles();//不知直接再进目录
            if (files != null && files.length > 0) {
                for (File file : files) {
                    search(file, filename);
                }
            }
        }
    }
}

```

使用递归实现

### 删除非空文件夹

功能实现：



```
import java.io.File;

public class Test {
    public static void main(String[] args) {

    }
    public static void delete(File dir){
        if(dir == null || dir.exists()){//极端情况
            return;
        }
        if (dir.isFile()){//文件直接删
            dir.delete();
            return;
        }
       File[] files = dir.listFiles();//是文件夹，然后获取
        if(files == null){//空的
            return;
        }
        for(File file : files){
            if (file.isFile()){
                file.delete();//遍历里面，文件直接删
            }
            else {
                delete(file);//递归
            }
        }
        dir.delete();//把自己删除
    }
}

```

# IO流

I是指input把数据读到内存中去

o是指output负责把写数据出去

应用场景：

- 展示
- 最高分
- 文件拷贝
- 通信软件

分类和体系：

- 输入流
- 输出流

或者

- 字节流 所有类型的文件
- 字符流 纯文本文件

字节输入流：InputStream

字节输出流：OutputStream

字符输入流：Reader

字符输出流：Writer

这四个都是抽象类，要找他们的实现类

前面都带着File就是他们的实现类

## 字节流

### FileInputStream

是文件字节输入流

就是从文件中读取内容到内存中去

#### 读取单个字节

```
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        InputStream is =   new FileInputStream(("ab.txt"));//简化推荐
        int b1 = is.read();
        System.out.println((char)b1);//每次读取一个字节，空返回-1

    }
}
```

简单案例

文件的完全读取，使用循环

```
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        InputStream is =   new FileInputStream(("ab.txt"));//简化推荐
        int b;
        while ((b = is.read())!=-1){
            System.out.print((char) b);
        }
    }
}
```

等号先看右边，先看小括号里面的

这种读取性能很差，每次只读取一个字节。读取汉字会乱码

流使用完后必须关闭，释放系统资源

#### 读取多个字节

```
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        InputStream is = new FileInputStream("ab.txt");
        byte[] buffer = new byte[1024*8];
        int len = is.read(buffer);
        String rs = new String(buffer,0,len);////读取多少，用多少
        System.out.println(rs);//转成字符
        System.out.println(len);//字节数
    }
}
```

在这里面读取多少用多少

读取完毕返回-1

优化：

```
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        InputStream is = new FileInputStream("ab.txt");
        byte[] buffer = new byte[1024*8];
        int len;
        while ((len = is.read(buffer))!= -1){
            String rs = new String(buffer,0,len);
            System.out.print(rs);
        }
        is.close();
    }
}
```

但是这种方式还是不能避免汉字乱码的问题

只要这个桶足够大，就可以避免汉字乱码的问题

或者是让文件的大小和读取的byte相同，只适合相对来说比较小的方法

### FileOutputStream

简单的文件输入

```
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        OutputStream os = new FileOutputStream("qq.txt");
        os.write(98);//每次写一个字节
        os.close();
    }
}
```

直接输入汉字会出现乱码，汉字代表三个字节，而这个方法稚只能写1个

这个是写一部分

```
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        OutputStream os = new FileOutputStream("qq.txt");
        byte[] bytes =  "我爱你爱大米".getBytes();
        os.write(bytes,0,15);
    }
}
```

每次写的时候都会将原来的内容清空

```
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        OutputStream os = new FileOutputStream("qq.txt",true);
        byte[] bytes =  "我爱你爱大米".getBytes();
        os.write(bytes,0,15);
    }
}
```

这样的话就是一个追加数据的地方

换行

```
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        OutputStream os = new FileOutputStream("qq.txt",true);
        byte[] bytes =  "我爱你爱大米".getBytes();
        os.write(bytes,0,15);
        os.close();
        os.write("\r\n".getBytes());
    }
}
```

这样就可以进行换行的操作了

### 文件复制

```
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        InputStream is= new FileInputStream("ab.txt");
        OutputStream os = new FileOutputStream("ab2.txt");
        byte[] buffer = new byte[1024];
        int len;
        while ((len = is.read(buffer))!=-1){
            os.write(buffer,0,len);
        }
        os.close();
        is.close();
    }
}
```

后开的流先关，先开的流后关

**这个文件可以复制一切的文件**

源文件和复制文件的类型都一致就可以复制成功

### 释放资源的方式

try-catch-finally

**在finally中的这个语句一定会执行的，除非jvm终止**

用来关闭io流等一系列资源

把close等一系列方法放到

```
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        InputStream is = null;
        OutputStream os = null;
        try {
            is = new FileInputStream("ab.txt");
            os = new FileOutputStream("ab2.txt");
            byte[] buffer = new byte[1024];
            int len;
            while ((len = is.read(buffer)) != -1) {
                os.write(buffer, 0, len);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            os.close();
            is.close();
        }
    }
}
```

实例演示，最后怕os和is出现空值的情况，可以加上if(!=null)的判断

这样的话，代码比较臃肿

try-with-resource

```
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        try(InputStream is = new FileInputStream("ab.txt");
            OutputStream os = new FileOutputStream("ab2.txt");) {
            byte[] buffer = new byte[1024];
            int len;
            while ((len = is.read(buffer)) != -1) {
                os.write(buffer, 0, len);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

这样的话定义在小括号里面运行完之后会自动的执行close方法，对流进行关闭

但是这里面只能定义资源变量

资源都会实现一个接口AutoCloseable的

## 字符流

适合读写文本文件的内容，读写文本文件的内容

### FileReader

在文件中的数据以字符的形式读取到内存中去

```
import java.io.*;

public class Test  {
    public static void main(String[] args) {
        try (Reader fr = new FileReader("ab.txt")){
            int c;
            while ((c = fr.read() )!=-1){
                System.out.print((char) c);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

读取中文的时候不会出现乱码

这样频繁的进行读，每次只读取一个，性能是比较差的

```
import java.io.*;

public class Test {
    public static void main(String[] args) {
        try (Reader fr = new FileReader("ab.txt")) {
            char[] buffer = new char[3];
            int len;
            while ((len = fr.read(buffer)) != -1) {
                System.out.println(new String(buffer, 0, len));
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}

```

这种方式是比较好的读取文本文件的方式

### FileWriter

在内存中的数据以字符的形式写出到文件中去

```
import java.io.*;

public class Test {
    public static void main(String[] args) {
        try (Writer fw = new FileWriter("aba.txt",true)) {
            fw.write("a");
            fw.write("沃茨aaaa",0,5);
            fw.write("\r\n");
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}


```

也是可以写字符数组的

换成true就是一个追加数据的管道

**写出数据后，必须刷新流或者关闭流，写出去的数据才能够生效**

都会先把数据写到缓冲区里面，然后把缓冲区，再通过系统调用进去

```
import java.io.*;

public class Test {
    public static void main(String[] args) {
        try (Writer fw = new FileWriter("aba.txt",true)) {
            fw.write("a");
            fw.write("沃茨aaaa",0,5);
            fw.write("\r\n");
            fw.flush();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}

```

这是刷新流，刷新完之后流还可以使用，

**在开发中使用close方法关闭流，也是包含刷新流这个操作的**

## 缓冲流

- 字节缓冲输入流
- 字节缓冲输出流
- 字符缓冲输入流
- 字符缓冲输出流

**对原始流进行包装，提高原始流读写数据的性能**

### 字节缓冲流

各自都自带8kb的缓冲池

```
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        InputStream is= new FileInputStream("ab.txt");
        InputStream bis = new BufferedInputStream(is);
        OutputStream os = new FileOutputStream("ab2.txt");
        OutputStream bos = new BufferedOutputStream(os);
        byte[] buffer = new byte[1024];
        int len;
        while ((len = bis.read(buffer))!=-1){
            bos.write(buffer,0,len);
        }
        
    }
}
```

对FileInputStream和FileOutputStream进行包装，可以提高其效率

```
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        InputStream is= new FileInputStream("ab.txt");
        InputStream bis = new BufferedInputStream(is,8192*2);
        OutputStream os = new FileOutputStream("ab2.txt");
        OutputStream bos = new BufferedOutputStream(os);
        byte[] buffer = new byte[1024];
        int len;
        while ((len = bis.read(buffer))!=-1){
            bos.write(buffer,0,len);
        }

    }
}
```

对缓冲流的池进行扩大

### 字符缓冲流

也是自带8kb（8192个字符）的字符缓冲池，提高字符输入输出读取字符的数据的性能

优化和前面的一样，但是不能用多态的写法，因为要使用新增的方法

```
import java.io.*;

public class Test {
    public static void main(String[] args) {
        try (FileReader fr = new FileReader("aba.txt");
             BufferedReader br = new BufferedReader(fr)) {
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}

```

这里新增了一个按照行读的方法readLine

字符缓冲输出流多了一个newline的换行的功能

```
import java.io.*;

public class Test {
    public static void main(String[] args) {
        // 需要写入文件的内容
        String[] lines = {
                "This is line 1.",
                "This is line 2.",
                "This is line 3."
        };

        try (FileWriter fw = new FileWriter("aba.txt", true);
             BufferedWriter bw = new BufferedWriter(fw)) {
            // 写入每一行内容到文件
            for (String line : lines) {
                bw.write(line);
                bw.newLine(); // 写入换行符
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}

```

案例：

```
import java.io.*;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Test {
    public static void main(String[] args) {
        List<String> data = new ArrayList<>();

        // 读取文件内容
        try (BufferedReader br = new BufferedReader(new FileReader("ab.txt"))) {
            String line;
            while ((line = br.readLine()) != null) {
                data.add(line);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }

        // 对读取的数据进行排序
        Collections.sort(data);

        // 将排序后的数据写入新的文件
        try (BufferedWriter bw = new BufferedWriter(new FileWriter("abbbb.txt"))) {
            for (String sortedLine : data) {
                bw.write(sortedLine);
                bw.newLine();
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}

```

## 转换流

代码的编码和文本文件的编码一致就不会出现乱码，不一致就会出现乱码

用来解决字符读取文本文本内容乱码的问题

先会获取文件的原始字节流，然后按照真实的字符集编码转成字符输入流

```
import java.io.*;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Test {
    public static void main(String[] args) throws IOException {
        try(InputStream is = new FileInputStream("ab.txt");
            Reader isr = new  InputStreamReader( is,"GBK");) {
        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        }
    }
}
```

字符输出转换流

- 使用String getBytes方法
- OutputStreamWriter

跟上面的差不多

```
import java.io.*;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Test {
    public static void main(String[] args) throws IOException {
        try(OutputStream is = new FileOutputStream("ab.txt");
            Writer isr = new OutputStreamWriter( is,"GBK");) {
        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        }
    }
}

```

## 打印流

PrintStream和PrintWriter

打印是高效的将数据打印出去，打印啥出去就是啥

```
import java.io.*;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Test {
    public static void main(String[] args) {
        List<String> data = new ArrayList<>();

        // 示例数据
        data.add("Banana");
        data.add("Apple");
        data.add("Cherry");

        // 将数据写入文件
        try (PrintStream ps = new PrintStream(new FileOutputStream("ab.txt"))) {
            for (String line : data) {
                ps.println(line);
            }
        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        }

        // 读取文件内容
        try (BufferedReader br = new BufferedReader(new FileReader("ab.txt"))) {
            String line;
            data.clear();  // 清空列表以便重新读取文件内容
            while ((line = br.readLine()) != null) {
                data.add(line);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }

        // 对读取的数据进行排序
        Collections.sort(data);

        // 将排序后的数据写入新的文件
        try (BufferedWriter bw = new BufferedWriter(new FileWriter("abbbb.txt"))) {
            for (String sortedLine : data) {
                bw.write(sortedLine);
                bw.newLine();
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}


```

实例其一

PrintWrite和PrintStream的用法是差不多的，就是多了个对字符操作的方法

## 数据流

做通信的时候，经常使用

### DateOutputStream

允许把数据和类型一并写出去

```
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        DataOutputStream dos=new DataOutputStream(new FileOutputStream("aaa.txt"));
        dos.writeInt(6);
    }
}
```

### DataInputStream

跟上面的输出流是相对应的，方法也是相对应的

```
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        DataOutputStream dos=new DataOutputStream(new FileOutputStream("aaa.txt"));
        dos.writeInt(6);
        dos.close();
        DataInputStream dis = new DataInputStream(new FileInputStream("aaa.txt"));
        int i  = dis.readInt();
        System.out.println(i);
        dis.close();

    }
}
```

**读和写的顺序一定要一致，否则会出现错误**

## 序列化流

序列化：把java对象写入到文件中去

反序列化：把java对象从文件中读取出来

```
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        BigStar b = new BigStar();
        ObjectOutputStream os=new ObjectOutputStream(new FileOutputStream("aaaa.txt"));
        os.writeObject(u);
    }
}
```

序列化

要序列化的对象，必须实现Serializable接口才可以

反序列化

```
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("aaaa.txt"));
        try {
            BigStar b2 =(BigStar)ois.readObject();
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }
    }
}
```

如果想某个变量不参与序列化的时候，要将那个变量设置成

```
private  transient String name;
```

这样就可以不参与序列化了
