---
title: Javase学习基础（1）
categories:
  - javase
  
abbrlink: 2701
date: 2024.4.23
tags: 
   - java

---

# java basic

## 1.面对对象

1.类和对象

属性，即可以设置的一些属性

方法即可以进行的一些行为

## 2.变量

java中有八种基本类型，这八种都是关键字，在设置变量时不能够用这些关键字。

这八种分别是：

整型 （4种）
字符型 （1种）
浮点型 （2种）
布尔型（1种）

### 1.整型

| 类型  | 缺省值 | 长度 | 范围                         |
| ----- | ------ | ---- | ---------------------------- |
| byte  | 0      | 8    | -128~127                     |
| short | 0      | 16   | -32768~32767                 |
| int   | 0      | 32   | -2147483648~2147483647       |
| long  | 0      | 64   | -9223372036854~9223372036854 |



### 2.字符型

char 类型用于存放一个字符，值用单引号表示（双引号表示字符串）

### 3.浮点型

**注意： 默认的小数值是double类型的**
所以 float f = 54.321会出现编译错误，因为54.321的默认类型是 double，其类型 长度为64，超过了float的长度32
在数字后面**加一个字母f**，直接把该数字声明成float类型
float f2 = 54.321**f**,

### 4.布尔型

布尔型表示真假，其长度为1

true false

### 5.Sting

### 字面型：

```java
public class HelloWorld {

	public static void main(String[] args) {
		String name = "盖伦";
		char a= 'c';

		//以下是转义字符
		char tab = '\t'; //制表符
		char carriageReturn = '\r'; //回车
		char newLine = '\n'; //换行
		char doubleQuote = '\"'; //双引号
		char singleQuote = '\''; //单引号
		char backslash = '\\'; //反斜杠
```

### 类型转换

规则：从小到大自动转，从大到小强制转

### 作用域

如果一个变量，是声明在一个方法上的，就叫做**参数**
参数的作用域即为该方法内的所有代码
其他方法不能访问该参数
类里面也不能访问该参数

声明在方法内的变量，叫做局部变量
其作用域在声明开始的位置，到其所处于的块结束位置

### final

准确的描述是 当一个变量被final修饰的时候，该变量**只有一次赋值的机会**

## 3.操作符

scanner操作符（向控制台输入数据）

```java
import java.util.Scanner;
 
public class HelloWorld {
    public static void main(String[] args) {
        Scanner s = new Scanner(System.in);
        float a = s.nextFloat();
        System.out.println("读取的浮点数的值是："+a);

    }
}
import java.util.Scanner;
Scanner s = new Scanner(System.in);
```

要先引用。

字符串时LIne

注意：需要注意的是，如果在通过nextInt()读取了整数后，再接着读取字符串，读出来的是回车换行:"\r\n",因为nextInt仅仅读取数字信息，而不会**读取**回车换行"\r\n".

所以，如果在业务上需要读取了整数后，接着读取字符串，那么就应该连续执行两次nextLine()，第一次是取走回车换行，第二次才是读取真正的字符串import java.util.Scanner;

```java
import java.util.Scanner;
  
public class HelloWorld {
    public static void main(String[] args) {
        Scanner s = new Scanner(System.in);
        int i = s.nextInt();
        System.out.println("读取的整数是"+ i);
        String rn = s.nextLine();
        String a = s.nextLine();
        System.out.println("读取的字符串是："+a);
    }
}

```

#### 算数

如果任何运算单元的长度都不超过int,那么运算结果就按照int来计算

```Java
public class HelloWorld {
	public static void main(String[] args) {
		byte a = 1;
		byte b= 2;
		byte c = (byte) (a+b); //虽然a b都是byte类型，但是运算结果是int类型，需要进行强制转换
		int d = a+b;
	}
}

```

### 逻辑运算符

&&与&

无论长路与还是短路与
两边的运算单元都是布尔值
都为真时，才为真
任意为假，就为假
区别
长路与 两侧，都会被运算
短路与 只要第一个是false，第二个就不进行运算了

||与|

无论长路或还是短路或
两边的运算单元都是布尔值
都为假时，才为假
任意为真，就为真
区别
长路或 两侧都会被运算
短路或 只要第一个是true的，第二个就不进行运算了

！

取反!
真变为假
假变为真

^

异或^
不同，返回真
相同，返回假

### 位操作符

| Integer.toBinaryString() | 一个整数的二进制表达 | [示例代码](https://how2j.cn/k/operator/operator-bitwise/270.html#step1066) |
| ------------------------ | -------------------- | ------------------------------------------------------------ |
| \|                       | 位或                 | [示例代码](https://how2j.cn/k/operator/operator-bitwise/270.html#step541) |
| &                        | 位与                 | [示例代码](https://how2j.cn/k/operator/operator-bitwise/270.html#step542) |
| ^                        | 异或                 | [示例代码](https://how2j.cn/k/operator/operator-bitwise/270.html#step543) |
| ~                        | 取非                 | [示例代码](https://how2j.cn/k/operator/operator-bitwise/270.html#step544) |
| << >>                    | 左移 右移            |                                                              |

#### 位或

5的二进制是101
6的二进制是110
所以 5|6 对每一位进行或运算，得到 111->7

#### 位与

5的二进制是101
6的二进制是110
所以 5&6 对每一位进行与运算，得到 100->4

#### 异或

5的二进制是101
6的二进制是110
所以 5^6 对每一位进行异或运算，得到 011->3

一些特别情况：
任何数和自己进行异或 都等于 0
任何数和0 进行异或 都等于自己

#### 取非

5 的二进制是 00000101
所以取反即为 11111010
这个二进制换算成十进制即为-6

#### 左移右移

左移：根据一个整数的二进制表达，将其每一位都向左移动，最右边一位补0
右移：根据一个整数的二进制表达，将其每一位都向右移动

### java三元操作符

表达式?值1:值2
如果表达式为真 返回值1 
如果表达式为假 返回值2

## 4.java控制流程

### if语句

```java
if(表达式1){
  表达式2；
}
 //如果有多个表达式，必须用大括弧包括起来
//如果只有一个表达式可以不用写括弧，看上去会简约一些
```

### if else语句

```java
public class HelloWorld {
	public static void main(String[] args) {

		boolean b = false;

		if (b)
			System.out.println("yes");
		else
			System.out.println("no");

	}
}

```

### else if 是多条件判断

```java
public class HelloWorld {
	public static void main(String[] args) {

		//如果只使用 if,会执行4次判断
		int i = 2;
		if (i==1)
			System.out.println(1);
		if (i==2)
			System.out.println(2);
		if (i==3)
			System.out.println(3);
		if (i==4)
			System.out.println(4);
		
		//如果使用else if, 一旦在18行，判断成立， 20行和22行的判断就不会执行了，节约了运算资源
		if (i==1)
			System.out.println(1);
		else if (i==2)
			System.out.println(2);
		else if (i==3)
			System.out.println(3);
		else if (i==4)
			System.out.println(4);		
		
	}
}
```

### swich语句

switch可以使用byte,short,int,char,String,enum

**注:** 每个表达式结束，都应该有一个break;
**注:** String在Java1.7之前是不支持的, Java从1.7开始支持switch用String的，编译后是把String转化为hash值，其实还是整数

### while和do while循环

| while    | 条件为true时 重复执行                 | [示例代码](https://how2j.cn/k/control-flow/control-flow-while/273.html#step560) |
| -------- | ------------------------------------- | ------------------------------------------------------------ |
| do while | 条件为true时 重复执行，至少会执行一次 |                                                              |

### for循环

和c语言一样 for 

```java
for (int j = 0; j < 10; j++)
```

### continue

继续进行下一次循环

### 结束循环

#### 结束当前循环：break

#### 使用boolean变量结束外部循环

```java
public class HelloWorld {
    public static void main(String[] args) {
        boolean breakout = false; //是否终止外部循环的标记
        for (int i = 0; i < 10; i++) {
 
            for (int j = 0; j < 10; j++) {
                System.out.println(i + ":" + j);
                if (0 == j % 2) {
                    breakout = true; //终止外部循环的标记设置为true
                    break;
                }
            }
            if (breakout) //判断是否终止外部循环
                break;
        }
 
    }
}
```

#### 使用标签结束外部循环

```java
public class HelloWorld {
    public static void main(String[] args) {
          
        //打印单数    
        outloop: //outloop这个标示是可以自定义的比如outloop1,ol2,out5
        for (int i = 0; i < 10; i++) {
             
            for (int j = 0; j < 10; j++) {
                System.out.println(i+":"+j);
                if(0==j%2) 
                    break outloop; //如果是双数，结束外部循环
            }
             
        }
         
    }
}
```

## 5.数组

### 声明数组-创建数组

```java
public class HelloWorld {
	public static void main(String[] args) {
		//声明一个引用
		int[] a; 
		//创建一个长度是5的数组，并且使用引用a指向该数组
		a = new int[5];
		
		int[] b = new int[5]; //声明的同时，指向一个数组
		
	}
}

```

数组的第一个数是从0开始的，**.length属性**用于访问一个数组的长度
数组访问下标范围是0到长度-1
一旦超过这个范围,就会产生数组下标越界异常

```java
public class HelloWorld {
	public static void main(String[] args) {
		int[] a; 
		a = new int[5];
		
		System.out.println(a.length); //打印数组的长度
		
		a[4]=100; //下标4，实质上是“第5个”，即最后一个 
		a[5]=101; //下标5，实质上是“第6个”，超出范围 ,产生数组下标越界异常
		
	}
}
```

随机获得整数

```
(int) (Math.random() * 100)
```

### 初始化数组

作为int类型的数组，默认值是0

```java
public class HelloWorld {
    public static void main(String[] args) {
        //写法一： 分配空间同时赋值
        int[] a = new int[]{100,102,444,836,3236};
 
        //写法二： 省略了new int[],效果一样
        int[] b = {100,102,444,836,3236};
         
        //写法三：同时分配空间，和指定内容
        //在这个例子里，长度是3，内容是5个，产生矛盾了
        //所以如果指定了数组的内容，就不能同时设置数组的长度
        int[] c = new int[3]{100,102,444,836,3236};
         
    }
}
```

### 排序

#### 选择排序

```java
public class HelloWorld {
    public static void main(String[] args) {
        int a [] = new int[]{18,62,68,82,65,9};
        //排序前，先把内容打印出来
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i] + " ");
        }
        System.out.println(" ");
        //选择法排序
     
        //第一步： 把第一位和其他所有位进行比较
        //如果发现其他位置的数据比第一位小，就进行交换
         
        for (int i = 1; i < a.length; i++) {
            if(a[i]<a[0]){  
                int temp = a[0];
                a[0] = a[i];
                a[i] = temp;
            }
        }
        //把内容打印出来
        //可以发现，最小的一个数，到了最前面
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i] + " ");
        }
        System.out.println(" ");
         
        //第二步： 把第二位的和剩下的所有位进行比较
        for (int i = 2; i < a.length; i++) {
            if(a[i]<a[1]){  
                int temp = a[1];
                a[1] = a[i];
                a[i] = temp;
            }
        }
        //把内容打印出来
        //可以发现，倒数第二小的数，到了第二个位置
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i] + " ");
        }
        System.out.println(" ");       
         
        //可以发现一个规律
        //移动的位置是从0 逐渐增加的
        //所以可以在外面套一层循环
         
        for (int j = 0; j < a.length-1; j++) {
            for (int i = j+1; i < a.length; i++) {
                if(a[i]<a[j]){  
                    int temp = a[j];
                    a[j] = a[i];
                    a[i] = temp;
                }
            }
        }
         
        //把内容打印出来
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i] + " ");
        }
        System.out.println(" ");       
    }
}
```

#### 冒泡排序

```java
public class HelloWorld {
    public static void main(String[] args) {
        int a [] = new int[]{18,62,68,82,65,9};
        //排序前，先把内容打印出来
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i] + " ");
        }
        System.out.println(" ");
        //冒泡法排序
      
        //第一步：从第一位开始，把相邻两位进行比较
        //如果发现前面的比后面的大，就把大的数据交换在后面
          
        for (int i = 0; i < a.length-1; i++) {
            if(a[i]>a[i+1]){  
                int temp = a[i];
                a[i] = a[i+1];
                a[i+1] = temp;
            }
        }
        //把内容打印出来
        //可以发现，最大的到了最后面
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i] + " ");
        }
        System.out.println(" ");
          
        //第二步： 再来一次，只不过不用比较最后一位
        for (int i = 0; i < a.length-2; i++) {
            if(a[i]>a[i+1]){  
                int temp = a[i];
                a[i] = a[i+1];
                a[i+1] = temp;
            }
        }
        //把内容打印出来
        //可以发现，倒数第二大的到了倒数第二个位置
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i] + " ");
        }
        System.out.println(" ");       
          
        //可以发现一个规律
        //后边界在收缩
        //所以可以在外面套一层循环
          
        for (int j = 0; j < a.length; j++) {
            for (int i = 0; i < a.length-j-1; i++) {
                if(a[i]>a[i+1]){  
                    int temp = a[i];
                    a[i] = a[i+1];
                    a[i+1] = temp;
                }
            }
        }
          
        //把内容打印出来
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i] + " ");
        }
        System.out.println(" ");       
    }
}
```

### 增强型for循环

注：增强型for循环只能用来取值，却不能用来修改数组里的值

```java
public class HelloWorld {
    public static void main(String[] args) {
        int values [] = new int[]{18,62,68,82,65,9};
        //常规遍历
        for (int i = 0; i < values.length; i++) {
            int each = values[i];
            System.out.println(each);
        }
         
        //增强型for循环遍历
        for (int each : values) {
            System.out.println(each);
        }
         
    }
}
```

### 复制数组

把一个数组的值，复制到另一个数组中

 

System.arraycopy(src, srcPos, dest, destPos, length)
src: 源数组
srcPos: 从源数组复制数据的起始位置
dest: 目标数组
destPos: 复制到目标数组的起始位置
length: 复制的长度

```java
public class HelloWorld {
    public static void main(String[] args) {
        int a [] = new int[]{18,62,68,82,65,9};
         
        int b[] = new int[3];//分配了长度是3的空间，但是没有赋值
         
        //通过数组赋值把，a数组的前3位赋值到b数组
         
        //方法一： for循环
         
        for (int i = 0; i < b.length; i++) {
            b[i] = a[i];
        }
        
        //方法二: System.arraycopy(src, srcPos, dest, destPos, length)
        //src: 源数组
        //srcPos: 从源数组复制数据的起始位置
        //dest: 目标数组
        //destPos: 复制到目标数组的启始位置
        //length: 复制的长度       
        System.arraycopy(a, 0, b, 0, 3);
         
        //把内容打印出来
        for (int i = 0; i < b.length; i++) {
            System.out.print(b[i] + " ");
        }
 
    }
}
```

练习复制数组：

```java
import javax.management.MXBean;
import java.util.Arrays;
public class Arrs {
 
    public static void main(String[] args) {
 
        int [] arr_0;
        int [] arr_1;
        while (true)
        {
            int temp = (int) (Math.random() * 10) + 1;
            if (temp >5 && temp < 10) {
                arr_0 = new int[temp];
                break;
            }
        }
 
        while (true)
        {
            int temp = (int) (Math.random() * 10) + 1;
            if (temp >5 && temp < 10) {
                arr_1 = new int[temp];
                break;
            }
        }
 
        for (int i = 0; i < arr_0.length; i++)
            arr_0[i] = (int) (Math.random() * 100) + 1;
 
        for (int i = 0; i < arr_0.length; i++)
            arr_1[i] = (int) (Math.random() * 100) + 1;
 
        int [] new_arr = new int[arr_0.length + arr_1.length];
        System.arraycopy(arr_0,0,new_arr,0,arr_0.length);
        System.arraycopy(arr_1,0,new_arr,arr_0.length,arr_1.length);
 
        System.out.println(arr_0.length);
        System.out.println(arr_1.length);
        System.out.println(Arrays.toString(arr_0));
        System.out.println(Arrays.toString(arr_1));
        System.out.println(Arrays.toString(new_arr));
        System.out.println("Done");
    }
}
```

### 二维数组

这是一个**二维数组**，里面的每一个元素，都是一个一维数组
所以二维数组又叫数组的数组。

```java
int b[][] = new int[][]{
   {1,2,3},
   {4,5,6},
   {7,8,9}
};
```

初始化二维数组

```java
public class HelloWorld {
    public static void main(String[] args) {
       //初始化二维数组，
       int[][] a = new int[2][3]; //有两个一维数组，每个一维数组的长度是3
       a[1][2] = 5;  //可以直接访问一维数组，因为已经分配了空间
          
       //只分配了二维数组
       int[][] b = new int[2][]; //有两个一维数组，每个一维数组的长度暂未分配
       b[0]  =new int[3]; //必须事先分配长度，才可以访问
       b[0][2] = 5;
        
       //指定内容的同时，分配空间
       int[][] c = new int[][]{
               {1,2,4},
               {4,5},
               {6,7,8,9}
       };
 
    }
}
```

### Arrays

| 关键字       | 简介         | 示例代码                                                     |
| :----------- | :----------- | :----------------------------------------------------------- |
| copyOfRange  | 数组复制     | [示例代码](https://how2j.cn/k/array/array-arrays/516.html#step1059) |
| toString()   | 转换为字符串 | [示例代码](https://how2j.cn/k/array/array-arrays/516.html#step2225) |
| sort         | 排序         | [示例代码](https://how2j.cn/k/array/array-arrays/516.html#step1063) |
| binarySearch | 搜索         | [示例代码](https://how2j.cn/k/array/array-arrays/516.html#step1060) |
| equals       | 判断是否相同 | [示例代码](https://how2j.cn/k/array/array-arrays/516.html#step1061) |
| fill         | 填充         |                                                              |

数组复制

System.arraycopy，需要事先准备好目标数组，并分配长度。 copyOfRange 只需要源数组就就可以了，通过返回值，就能够得到目标数组了。
除此之外，需要注意的是 copyOfRange 的**第3个参数**，表示源数组的结束位置，是**取不到的**。

```java
import java.util.Arrays;
 
public class HelloWorld {
    public static void main(String[] args) {
        int a[] = new int[] { 18, 62, 68, 82, 65, 9 };
 
        // copyOfRange(int[] original, int from, int to)
        // 第一个参数表示源数组
        // 第二个参数表示开始位置(取得到)
        // 第三个参数表示结束位置(取不到)
        int[] b = Arrays.copyOfRange(a, 0, 3);
 
        for (int i = 0; i < b.length; i++) {
            System.out.print(b[i] + " ");
        }
 
    }
}
```

转换为字符串

Arrays提供了一个toString()方法，直接把一个数组，转换为字符串，这样方便观察数组的内容

```java
import java.util.Arrays;
  
public class HelloWorld {
    public static void main(String[] args) {
        int a[] = new int[] { 18, 62, 68, 82, 65, 9 };
        String content = Arrays.toString(a);
        System.out.println(content);
  
    }
}
```

排序

```java
import java.util.Arrays;
  
public class HelloWorld {
    public static void main(String[] args) {
        int a[] = new int[] { 18, 62, 68, 82, 65, 9 };
        System.out.println("排序之前 :");
        System.out.println(Arrays.toString(a));
        Arrays.sort(a);
        System.out.println("排序之后:");
        System.out.println(Arrays.toString(a));
  
    }
}
```

搜索

需要注意的是，使用binarySearch进行查找之前，必须使用sort进行排序
如果数组中有多个相同的元素，查找结果是不确定的

```java
import java.util.Arrays;
 
public class HelloWorld {
    public static void main(String[] args) {
        int a[] = new int[] { 18, 62, 68, 82, 65, 9 };
 
        Arrays.sort(a);
 
        System.out.println(Arrays.toString(a));
        //使用binarySearch之前，必须先使用sort进行排序
        System.out.println("数字 62出现的位置:"+Arrays.binarySearch(a, 62));
    }
}
```

判断是否相同

```java
import java.util.Arrays;
 
public class HelloWorld {
    public static void main(String[] args) {
        int a[] = new int[] { 18, 62, 68, 82, 65, 9 };
        int b[] = new int[] { 18, 62, 68, 82, 65, 8 };
 
        System.out.println(Arrays.equals(a, b));
    }
}
```

填充

使用同一个值，填充整个数组

```java
import java.util.Arrays;
  
public class HelloWorld {
    public static void main(String[] args) {
        int a[] = new int[10];
  
        Arrays.fill(a, 5);
  
        System.out.println(Arrays.toString(a));
  
    }
}
```

## 6.类和对象

### 引用

引用的概念，如果一个变量的类型是 类类型，而非基本类型，那么该变量又叫做引用。

```java
Hero h = new Hero();
```

h这个变量是Hero类型，又叫做引用
=的意思指的h这个引用**代表**右侧创建的对象
“**代表**” 在面向对象里，又叫做“**指向**”

引用有多个，但是对象只有一个。
在这个例子里，所有引用都指向了同一个对象。
对象就像 "房产"， 引用就像"房产证"
房产证的复印件可以有多张，但是真正的"房产" 只有这么一处

一个引用，多个对象时，只有最后一个能成功指向

### 继承

```java
public class Weapon extends Item{
    int damage; //攻击力
     
    public static void main(String[] args) {
        Weapon infinityEdge = new Weapon();
        infinityEdge.damage = 65; //damage属性在类Weapon中新设计的
         
        infinityEdge.name = "无尽之刃";//name属性，是从Item中继承来的，就不需要重复设计了
        infinityEdge.price = 3600;
         
    }
     
}
```

通过继承别的类的属性，不需要重复设计。

### 方法重载

方法名是一样的，但是参数类型不一样
在调用方法attack的时候，会根据传递的参数类型以及数量，自动调用对应的方法

```java
public class ADHero extends Hero {
    public void attack() {
        System.out.println(name + " 进行了一次攻击 ，但是不确定打中谁了");
    }
 
    public void attack(Hero h1) {
        System.out.println(name + "对" + h1.name + "进行了一次攻击 ");
    }
 
    public void attack(Hero h1, Hero h2) {
        System.out.println(name + "同时对" + h1.name + "和" + h2.name + "进行了攻击 ");
    }
 
    public static void main(String[] args) {
        ADHero bh = new ADHero();
        bh.name = "赏金猎人";
 
        Hero h1 = new Hero();
        h1.name = "盖伦";
        Hero h2 = new Hero();
        h2.name = "提莫";
 
        bh.attack(h1);
        bh.attack(h1, h2);
    }
 
}
```

### 构造方法

方法名和类名一样（包括大小写）
**没有返回类型**
实例化一个对象的时候，必然调用构造方法

一旦提供了一个有参的构造方法
同时又**没有显式**的提供一个无参的构造方法
那么默认的无参的构造方法，就“木有了“

### this

**this即代表当前对象**

```
//直接打印对象，会显示该对象在内存中的虚拟地址
    ``//格式：Hero@c17164 c17164即虚拟地址，每次执行，得到的地址不一定一样
    而调用行为则出现相同的地址
```

属性名和变量名不能一样

参数名不得不使用其他变量名

用this即可解决问题

```java
public class Hero {
     
    String name; //姓名
     
    float hp; //血量
     
    float armor; //护甲
     
    int moveSpeed; //移动速度
 
    //参数名和属性名一样
    //在方法体中，只能访问到参数name
    public void setName1(String name){
        name = name;
    }
     
    //为了避免setName1中的问题，参数名不得不使用其他变量名
    public void setName2(String heroName){
        name = heroName;
    }
     
    //通过this访问属性
    public void setName3(String name){
        //name代表的是参数name
        //this.name代表的是属性name
        this.name = name;
    }
     
    public static void main(String[] args) {
        Hero  h =new Hero();
         
        h.setName1("teemo");
        System.out.println(h.name);
         
        h.setName2("garen");
        System.out.println(h.name);    
         
        h.setName3("死歌");
        System.out.println(h.name);    
    }
     
}

```

### 传参

传参分为**基本类型传参**和**类类型传参**

基本：

基本类型传参
在方法内，无法修改方法外的基本类型参数

```java
public class Hero {
        
    String name; //姓名
        
    float hp; //血量
        
    float armor; //护甲
        
    int moveSpeed; //移动速度
     
    public Hero(){
         
    }
     
    //回血
    public void huixue(int xp){
        hp = hp + xp;
        //回血完毕后，血瓶=0
        xp=0;
    }
      
    public Hero(String name,float hp){
        this.name = name;
        this.hp = hp;
    }
 
    public static void main(String[] args) {
        Hero teemo =  new Hero("提莫",383);
        //血瓶，其值是100
        int xueping = 100;
         
        //提莫通过这个血瓶回血
         
        teemo.huixue(xueping);
         
        System.out.println(xueping);
         
    }
      
}
```

引用和=

果一个变量是基本类型
比如 int hp = 50;
我们就直接管hp叫变量
**=表示赋值的意思**。
如果一个变量是类类型
比如 Hero h = new Hero();
我们就管h叫做**引用**。
**=不再是赋值的意思**
**=表示指向的意思**
比如 Hero h = new Hero();
这句话的意思是
引用h，指向一个Hero对象

类类型传参：

**类类型又叫引用**
第24行的引用 **teemo**与 第17行的引用**hero**，**是不同的引用**
通过调用garen.attack(teemo, 100); 使得这**两个引用都指向了同一个对象**
所以在第18行hero.hp = hero.hp - damage; 就使得该对象的hp值，发生了变化
因此第25行，打印该对象的Hp值就是变化后的值

```java
public class Hero {
 
    String name; // 姓名
 
    float hp; // 血量
 
    float armor; // 护甲
 
    int moveSpeed; // 移动速度
 
    public Hero(String name, float hp) {
        this.name = name;
        this.hp = hp;
    }
 
    // 攻击一个英雄，并让他掉damage点血
    public void attack(Hero hero, int damage) {
        hero.hp = hero.hp - damage;
    }
 
    public static void main(String[] args) {
        Hero teemo = new Hero("提莫", 383);
        Hero garen = new Hero("盖伦", 616);
        garen.attack(teemo, 100);
        System.out.println(teemo.hp);
    }
 
}
```

### 包

package

把比较接近的类，规划在同一个包下

在最开始的地方声明该类所处于的包名

使用同一个包下的其他类，直接使用即可
但是要使用其他包下的类，必须import

```java
package charactor;
 
//Weapon类在其他包里，使用必须进行import
import property.Weapon;
 
public class Hero {
        
    String name; //姓名
        
    float hp; //血量
        
    float armor; //护甲
        
    int moveSpeed; //移动速度
     
    //装备一把武器
    public void equip(Weapon w){
         
    }
        
}
```

### 访问修饰符

成员变量有四种修饰符

**private** 私有的
**package**/friendly/default 不写
**protected** 受保护的
**public** 公共的

类之间的关系

**自身：**指的是Hero自己
**同包子类：**ADHero这个类是Hero的子类，并且和Hero处于**同一个包下**
**不同包子类：**Support这个类是Hero的子类，但是在**另一个包下**
**同包类：** GiantDragon 这个类和Hero是**同一个包**，但是彼此**没有继承关系**
**其他类：**Item这个类，**在不同包**，也没有继承关系的类

![总结](https://stepimagewm.how2j.cn/612.png)

### 类属性

当一个属性被**static**修饰的时候，就叫做**类属性**，又叫做**静态属性**
当一个属性被声明成类属性，那么**所有的对象，都共享一个值**
与对象属性对比：
不同对象的 对象属性 的值都可能不一样。
比如盖伦的hp 和 提莫的hp 是不一样的。
但是所有对象的类属性的值，都是一样的

如果一个属性声明成类属性，那么所有的对象，都共享这么一个值

所以类属性是可以被用作所有对象都一致的

### 类方法

**类方法：** 又叫做静态方法

**对象方法：** 又叫实例方法，非静态方法

访问一个对象方法，必须**建立在有一个对象**的前提的基础上
访问类方法，**不需要对象**的存在，直接就访问

```java
package charactor;
 
public class Hero {
    public String name;
    protected float hp;
 
    //实例方法,对象方法，非静态方法
    //必须有对象才能够调用
    public void die(){
        hp = 0;
    }
     
    //类方法，静态方法
    //通过类就可以直接调用
    public static void battleWin(){
        System.out.println("battle win");
    }
     
    public static void main(String[] args) {
           Hero garen =  new Hero();
           garen.name = "盖伦";
           //必须有一个对象才能调用
           garen.die();
            
           Hero teemo =  new Hero();
           teemo.name = "提莫";
            
           //无需对象，直接通过类调用
           Hero.battleWin();
         
    }
}
```

### 单例模式

```java
package charactor;
 
public class GiantDragon {
 
    //私有化构造方法使得该类无法在外部通过new 进行实例化
    private GiantDragon(){
         
    }
 
    //准备一个类属性，指向一个实例化对象。 因为是类属性，所以只有一个
 
    private static GiantDragon instance = new GiantDragon();
     
    //public static 方法，提供给调用者获取12行定义的对象
    public static GiantDragon getInstance(){
        return instance;
    }
     
}
```

### 枚举

枚举enum是一种特殊的类(还是类)，使用枚举可以很方便的定义常

```java
public class HelloWorld {
    public static void main(String[] args) {
        Season season = Season.SPRING;
        switch (season) {
        case SPRING:
            System.out.println("春天");
            break;
        case SUMMER:
            System.out.println("夏天");
            break;
        case AUTUMN:
            System.out.println("秋天");
            break;
        case WINTER:
            System.out.println("冬天");
            break;
        }
    }
}枚举enum是一种特殊的类(还是类)，使用枚举可以很方便的定义常
```

```java
public enum Season {
    SPRING,SUMMER,AUTUMN,WINTER
}
```

## 7.接口和继承

#### super

实例化子类，父类的构造方法一定会被调用

**其父类的构造方法也会被调用**
**并且是父类构造方法先调用**
**子类构造方法会默认调用父类的 无参的构造方法**

父类显式提供两个构造方法

分别是无参的构造方法和带一个参数的构造方法

**使用关键字super 显式调用父类带参的构造方法**

#### object

声明一个类的时候，默认是继承了Object
public class Hero **extends Object**,object是所有类的父类

Object类提供一个toString方法，所以所有的类都有toString方法
toString()的意思是返回当前对象的**字符串表达**

通过 System.out.println 打印对象就是打印该对象的toString()返回值

当一个对象没有任何引用指向的时候，它就满足垃圾回收的条件

当它被垃圾回收的时候，它的finalize() 方法就会被调用。

```java
package charactor;
  
public class Hero {
    public String name;
    protected float hp;
      
    public String toString(){
        return name;
    }
     
    public void finalize(){
        System.out.println("这个英雄正在被回收");
    }
      
    public static void main(String[] args) {
        //只有一引用
        Hero h;
        for (int i = 0; i < 100000; i++) {
            //不断生成新的对象
            //每创建一个对象，前一个对象，就没有引用指向了
            //那些对象，就满足垃圾回收的条件
            //当，垃圾堆积的比较多的时候，就会触发垃圾回收
            //一旦这个对象被回收，它的finalize()方法就会被调用
            h = new Hero();
        }
 
    }
}
```

#### equals() 用于判断两个对象的内容是否相同

```java
package charactor;
  
public class Hero {
    public String name;
    protected float hp;
      
    public boolean equals(Object o){
        if(o instanceof Hero){
            Hero h = (Hero) o;
            return this.hp == h.hp;
        }
        return false;
    }
      
    public static void main(String[] args) {
        Hero h1= new Hero();
        h1.hp = 300;
        Hero h2= new Hero();
        h2.hp = 400;
        Hero h3= new Hero();
        h3.hp = 300;
         
        System.out.println(h1.equals(h2));
        System.out.println(h1.equals(h3));
    }
}
```

#### ==

用于判断两个引用，是否指向了同一个对象

#### hashcode

hashCode方法返回一个对象的哈希值

#### final

当Hero被修饰成final的时候，表示Hero不能够被继承

Hero的useItem方法被修饰成final,那么该方法在ADHero中，不能够被重写

final修饰基本类型变量，表示该变量只有一次赋值机会

inal修饰引用
h引用被修饰成final，表示该引用只有**1**次指向对象的机会
所以17行会出现编译错误
但是，依然通过h引用修改对象的属性值hp，因为hp并没有final修饰

#### 抽象类

在类中声明一个方法，这个方法没有实现体，是一个“空”方法

这样的方法就叫抽象方法，使用修饰符“abstract"

当一个类有抽象方法的时候，该类必须被声明为抽象类

```java
package charactor;
 
public abstract class Hero {
    String name;
 
    float hp;
 
    float armor;
 
    int moveSpeed;
 
    public static void main(String[] args) {
 
    }
 
    // 抽象方法attack
    // Hero的子类会被要求实现attack方法
    public abstract void attack();
 
}
```

```java
package charactor;
 
public class ADHero extends Hero implements AD {
 
    public void physicAttack() {
        System.out.println("进行物理攻击");
    }
 
    @Override
    public void attack() {
        physicAttack();
    }
 
}
```

Hero类可以在不提供抽象方法的前提下，声明为抽象类
一旦一个类被声明为抽象类，就不能够被直接实例化

区别1：
子类只能继承一个抽象类，不能继承多个
子类可以实现**多个**接口
区别2：
抽象类可以定义
public,protected,package,private
静态和非静态属性
final和非final属性
但是接口中声明的属性，只能是
public
静态
final的
即便没有显式的声明

#### 内部类

内部类分为四种：
非静态内部类
静态内部类
匿名类
本地类

##### 非静态内部类

```java
package charactor;
 
public class Hero {
    private String name; // 姓名
 
    float hp; // 血量
 
    float armor; // 护甲
 
    int moveSpeed; // 移动速度
 
    // 非静态内部类，只有一个外部类对象存在的时候，才有意义
    // 战斗成绩只有在一个英雄对象存在的时候才有意义
    class BattleScore {
        int kill;
        int die;
        int assit;
 
        public void legendary() {
            if (kill >= 8)
                System.out.println(name + "超神！");
            else
                System.out.println(name + "尚未超神！");
        }
    }
 
    public static void main(String[] args) {
        Hero garen = new Hero();
        garen.name = "盖伦";
        // 实例化内部类
        // BattleScore对象只有在一个英雄对象存在的时候才有意义
        // 所以其实例化必须建立在一个外部类对象的基础之上
        BattleScore score = garen.new BattleScore();
        score.kill = 9;
        score.legendary();
    }
 
}
```

##### 静态内部类

```java
package charactor;
  
public class Hero {
    public String name;
    protected float hp;
  
    private static void battleWin(){
        System.out.println("battle win");
    }
     
    //敌方的水晶
    static class EnemyCrystal{
        int hp=5000;
         
        //如果水晶的血量为0，则宣布胜利
        public void checkIfVictory(){
            if(hp==0){
                Hero.battleWin();
                 
                //静态内部类不能直接访问外部类的对象属性
                System.out.println(name + " win this game");
            }
        }
    }
     
    public static void main(String[] args) {
        //实例化静态内部类
        Hero.EnemyCrystal crystal = new Hero.EnemyCrystal();
        crystal.checkIfVictory();
    }
  
}
```

在一个类里面声明一个静态内部类
比如敌方水晶，当敌方水晶没有血的时候，己方所有英雄都取得胜利，而不只是某一个具体的英雄取得胜利。
与非静态内部类不同，**静态内部类**水晶类的实例化 **不需要一个外部类的实例为基础**，可以直接实例化
语法：**new 外部类.静态内部类();**
因为没有一个外部类的实例，所以在静态内部类里面**不可以访问外部类的实例属性和方法**
除了可以访问外部类的**私有静态成员外**，静态内部类和普通类没什么大的区别

##### 匿名类

匿名类指的是在**声明一个类的同时实例化它**，使代码更加简洁精练
通常情况下，要使用一个接口或者抽象类，都必须创建一个子类

有的时候，为了快速使用，直接实例化一个抽象类，并“**当场**”实现其抽象方法。
既然实现了抽象方法，那么就是一个新的类，只是这个类，没有命名。
这样的类，叫做匿名类

```java
package charactor;
   
public abstract class Hero {
    String name; //姓名
          
    float hp; //血量
          
    float armor; //护甲
          
    int moveSpeed; //移动速度
      
    public abstract void attack();
      
    public static void main(String[] args) {
          
        ADHero adh=new ADHero();
        //通过打印adh，可以看到adh这个对象属于ADHero类
        adh.attack();
        System.out.println(adh);
          
        Hero h = new Hero(){
            //当场实现attack方法
            public void attack() {
                System.out.println("新的进攻手段");
            }
        };
        h.attack();
        //通过打印h，可以看到h这个对象属于Hero$1这么一个系统自动分配的类名
          
        System.out.println(h);
    }
      
}
```

##### 本地类

本地类可以理解为有名字的匿名类
内部类与匿名类不一样的是，内部类必须声明在成员的位置，即与属性和方法平等的位置。
本地类和匿名类一样，直接声明在代码块里面，可以是主方法，for循环里等等地方

```java
package charactor;
   
public abstract class Hero {
    String name; //姓名
          
    float hp; //血量
          
    float armor; //护甲
          
    int moveSpeed; //移动速度
      
    public abstract void attack();
      
    public static void main(String[] args) {
          
        //与匿名类的区别在于，本地类有了自定义的类名
        class SomeHero extends Hero{
            public void attack() {
                System.out.println( name+ " 新的进攻手段");
            }
        }
         
        SomeHero h  =new SomeHero();
        h.name ="地卜师";
        h.attack();
    }
      
}
```

##### 默认方法

默认方法是JDK8新特性，指的是接口也可以提供具体方法了，而不像以前，只能提供抽象方法.

### 8.数字和字符串

#### 数字封装类

数字封装类有
Byte,Short,Integer,Long,Float,Double
这些类都是抽象类Number的子类

不需要调用构造方法，**通过=符号****自动**把 基本类型 转换为 类类型 就叫装箱

```java
package digit;
 
public class TestNumber {
 
    public static void main(String[] args) {
        int i = 5;
 
        //基本类型转换成封装类型
        Integer it = new Integer(i);
         
        //自动转换就叫装箱
        Integer it2 = i;
         
    }
}
```



不需要调用Integer的intValue方法，通过=就自动转换成int类型，就叫拆箱

```java
package digit;
  
public class TestNumber {
  
    public static void main(String[] args) {
        int i = 5;
  
        Integer it = new Integer(i);
          
        //封装类型转换成基本类型
        int i2 = it.intValue();
         
        //自动转换就叫拆箱
        int i3 = it;
          
    }
}
```



nt的最大值可以通过其对应的封装类Integer.MAX_VALUE获取



#### 字符串

##### 数字转字符串：

方法1： 使用String类的静态方法valueOf
方法2： 先把基本类型装箱为对象，然后调用对象的toString

```java
package digit;
  
public class TestNumber {
  
    public static void main(String[] args) {
        int i = 5;
         
        //方法1
        String str = String.valueOf(i);
         
        //方法2
        Integer it = i;
        String str2 = it.toString();
         
    }
}
```

##### 调用Integer的静态方法parseInt

```java
package digit;
  
public class TestNumber {
  
    public static void main(String[] args) {
 
        String str = "999";
         
        int i= Integer.parseInt(str);
         
        System.out.println(i);
         
    }
}
```

#### java.lang.Math

数学方法

```java
package digit;
  
public class TestNumber {
  
    public static void main(String[] args) {
        float f1 = 5.4f;
        float f2 = 5.5f;
        //5.4四舍五入即5
        System.out.println(Math.round(f1));
        //5.5四舍五入即6
        System.out.println(Math.round(f2));
         
        //得到一个0-1之间的随机浮点数（取不到1）
        System.out.println(Math.random());
         
        //得到一个0-10之间的随机整数 （取不到10）
        System.out.println((int)( Math.random()*10));
        //开方
        System.out.println(Math.sqrt(9));
        //次方（2的4次方）
        System.out.println(Math.pow(2,4));
         
        //π
        System.out.println(Math.PI);
         
        //自然常数
        System.out.println(Math.E);
    }
}
```



#### 格式化

%s 表示字符串
%d 表示数字
%n 表示换行

```java
package digit;
  
public class TestNumber {
  
    public static void main(String[] args) {
 
        String name ="盖伦";
        int kill = 8;
        String title="超神";
         
        //直接使用+进行字符串连接，编码感觉会比较繁琐，并且维护性差,易读性差
        String sentence = name+ " 在进行了连续 " + kill + " 次击杀后，获得了 " + title +" 的称号";
         
        System.out.println(sentence);
         
        //使用格式化输出
        //%s表示字符串，%d表示数字,%n表示换行
        String sentenceFormat ="%s 在进行了连续 %d 次击杀后，获得了 %s 的称号%n";
        System.out.printf(sentenceFormat,name,kill,title);
         

```

printf



 

```java
package digit;
  
public class TestNumber {
  
    public static void main(String[] args) {
 
        String name ="盖伦";
        int kill = 8;
        String title="超神";
         
        String sentenceFormat ="%s 在进行了连续 %d 次击杀后，获得了 %s 的称号%n";
        //使用printf格式化输出
        System.out.printf(sentenceFormat,name,kill,title);
        //使用format格式化输出
        System.out.format(sentenceFormat,name,kill,title);
         
    }
}
```

**换行符**就是另起一行 --- '\n' 换行（newline）
**回车符**就是回到一行的开头 --- '\r' 回车（return）
在eclipse里敲一个回车，实际上是**回车换行符**

```java
package digit;
  
import java.util.Locale;
   
public class TestNumber {
   
    public static void main(String[] args) {
        int year = 2020;
        //总长度，左对齐，补0，千位分隔符，小数点位数，本地化表达
          
        //直接打印数字
        System.out.format("%d%n",year);
        //总长度是8,默认右对齐
        System.out.format("%8d%n",year);
        //总长度是8,左对齐
        System.out.format("%-8d%n",year);
        //总长度是8,不够补0
        System.out.format("%08d%n",year);
        //千位分隔符
        System.out.format("%,8d%n",year*10000);
  
        //小数点位数
        System.out.format("%.2f%n",Math.PI);
          
        //不同国家的千位分隔符
        System.out.format(Locale.FRANCE,"%,.2f%n",Math.PI*10000);
        System.out.format(Locale.US,"%,.2f%n",Math.PI*10000);
        System.out.format(Locale.UK,"%,.2f%n",Math.PI*10000);
          
    }
}
     
```

##### 字符:

保存一个字符的时候使用char

char对应的封装类是Character

```java
package character;
 
public class TestChar {
 
    public static void main(String[] args) {
         
        System.out.println(Character.isLetter('a'));//判断是否为字母
        System.out.println(Character.isDigit('a')); //判断是否为数字
        System.out.println(Character.isWhitespace(' ')); //是否是空白
        System.out.println(Character.isUpperCase('a')); //是否是大写
        System.out.println(Character.isLowerCase('a')); //是否是小写
         
        System.out.println(Character.toUpperCase('a')); //转换为大写
        System.out.println(Character.toLowerCase('A')); //转换为小写
 
        String a = 'a'; //不能够直接把一个字符转换成字符串
        String a2 = Character.toString('a'); //转换为字符串
         
    }
}
```

length方法返回当前字符串的长度

|                              |                    |                                                              |
| ---------------------------- | ------------------ | ------------------------------------------------------------ |
| charAt                       | 获取字符           | [示例代码](https://how2j.cn/k/number-string/number-string-manipulate/325.html#step712) |
| toCharArray                  | 获取对应的字符数组 | [示例代码](https://how2j.cn/k/number-string/number-string-manipulate/325.html#step719) |
| subString                    | 截取子字符串       | [示例代码](https://how2j.cn/k/number-string/number-string-manipulate/325.html#step713) |
| split                        | 分隔               | [示例代码](https://how2j.cn/k/number-string/number-string-manipulate/325.html#step714) |
| trim                         | 去掉首尾空格       | [示例代码](https://how2j.cn/k/number-string/number-string-manipulate/325.html#step715) |
| toLowerCase toUpperCase      | 大小写             | [示例代码](https://how2j.cn/k/number-string/number-string-manipulate/325.html#step716) |
| indexOf lastIndexOf contains | 定位               | [示例代码](https://how2j.cn/k/number-string/number-string-manipulate/325.html#step717) |
| replaceAll replaceFirst      |                    |                                                              |

使用equals进行字符串内容的比较，必须大小写一致
equalsIgnoreCase，忽略大小写判断内容是否一致

```java
package character;
  
public class TestString {
  
    public static void main(String[] args) {
        String str1 = "the light";
         
        String start = "the";
        String end = "Ight";
         
        System.out.println(str1.startsWith(start));//以...开始
        System.out.println(str1.endsWith(end));//以...结束
          
    }
  
}
```

```java
package character;
  
public class TestString {
  
    public static void main(String[] args) {
        String str1 = "let there ";
 
        StringBuffer sb = new StringBuffer(str1); //根据str1创建一个StringBuffer对象
        sb.append("be light"); //在最后追加
         
        System.out.println(sb);
         
        sb.delete(4, 10);//删除4-10之间的字符
         
        System.out.println(sb);
         
        sb.insert(4, "there ");//在4这个位置插入 there
         
        System.out.println(sb);
         
        sb.reverse(); //反转
         
        System.out.println(sb);
 
    }
  
}
```

#### 时间日历

```java
package date;
 
//
import java.util.Date;
 
public class TestDate {
 
    public static void main(String[] args) {
 
        // 当前时间
        Date d1 = new Date();
        System.out.println("当前时间:");
        System.out.println(d1);
        System.out.println();
        // 从1970年1月1日 早上8点0分0秒 开始经历的毫秒数
        Date d2 = new Date(5000);
        System.out.println("从1970年1月1日 早上8点0分0秒 开始经历了5秒的时间");
        System.out.println(d2);
 
    }
}
```

 

```java
package date;
  
//
import java.util.Date;
  
public class TestDate {
  
    public static void main(String[] args) {
        //注意：是java.util.Date;
        //而非 java.sql.Date，此类是给数据库访问的时候使用的
        Date now= new Date();
        //打印当前时间
        System.out.println("当前时间:"+now.toString());
        //getTime() 得到一个long型的整数
        //这个整数代表 1970.1.1 08:00:00:000，每经历一毫秒，增加1
        System.out.println("当前时间getTime()返回的值是："+now.getTime());
          
        Date zero = new Date(0);
        System.out.println("用0作为构造方法，得到的日期是:"+zero);
          
    }
}
```

```
 System.out.println("System.currentTimeMillis() \t返回值: "+System.currentTimeMillis());
```

 

```java
package` `date;
 
//
import` `java.text.SimpleDateFormat;
import` `java.util.Date;
 
public` `class` `TestDate {
 
  ``public` `static` `void` `main(String[] args) {
     
    ``//y 代表年
    ``//M 代表月
    ``//d 代表日
    ``//H 代表24进制的小时
    ``//h 代表12进制的小时
    ``//m 代表分钟
    ``//s 代表秒
    ``//S 代表毫秒
    ``SimpleDateFormat sdf =``new` `SimpleDateFormat(``"yyyy-MM-dd HH:mm:ss SSS"` `);
    ``Date d= ``new` `Date();
    ``String str = sdf.format(d);
    ``System.out.println(``"当前时间通过 yyyy-MM-dd HH:mm:ss SSS 格式化后的输出: "``+str);
    
    ``SimpleDateFormat sdf1 =``new` `SimpleDateFormat(``"yyyy-MM-dd"` `);
    ``Date d1= ``new` `Date();
    ``String str1 = sdf1.format(d1);
    ``System.out.println(``"当前时间通过 yyyy-MM-dd 格式化后的输出: "``+str1);
    
  ``}
}
```

```java
package date;
  
//
import java.util.Calendar;
import java.util.Date;
  
public class TestDate {
  
    public static void main(String[] args) {
        //采用单例模式获取日历对象Calendar.getInstance();
        Calendar c = Calendar.getInstance();
          
        //通过日历对象得到日期对象
        Date d = c.getTime();
  
        Date d2 = new Date(0);
        c.setTime(d2); //把这个日历，调成日期 : 1970.1.1 08:00:00
    }
}
```

 

```java
package date;
 
import java.text.SimpleDateFormat;
//
import java.util.Calendar;
import java.util.Date;
 
public class TestDate {
 
    private static SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
 
    public static void main(String[] args) {
        Calendar c = Calendar.getInstance();
        Date now = c.getTime();
        // 当前日期
        System.out.println("当前日期：\t" + format(c.getTime()));
 
        // 下个月的今天
        c.setTime(now);
        c.add(Calendar.MONTH, 1);
        System.out.println("下个月的今天:\t" +format(c.getTime()));
 
        // 去年的今天
        c.setTime(now);
        c.add(Calendar.YEAR, -1);
        System.out.println("去年的今天:\t" +format(c.getTime()));
 
        // 上个月的第三天
        c.setTime(now);
        c.add(Calendar.MONTH, -1);
        c.set(Calendar.DATE, 3);
        System.out.println("上个月的第三天:\t" +format(c.getTime()));
 
    }
 
    private static String format(Date time) {
        return sdf.format(time);
    }
}
```

 



 

