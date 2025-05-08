---
title: javase 集合 Collection
categories:
  - javase
abbrlink: 2701
date: 2024.8.8
tags: 
   - java

---

#  Java 集合 basic

集合是一种容器，类似与数组。集合的大小是可变的

Collection 单列集合

Map 双列集合

单列每个元素只会包含一个值

双列每个元素会包含两个值，或者称为键值对

## 

## Collection

List接口和Set接口

- **List系列是有序 可重复 有索引的**

- **Set系列是无序的 不可重复的 无索引的**

  - Hashset无序
  - LinkedHashSet有序的
  - TreeSet按照大小默认升序排序

  

### 常用方法

```java
import java.util.ArrayList;
import java.util.Collection;

public class Test{
   public static void main(String[] args) {
       Collection<String> c = new ArrayList<>();//多态
       c.add("java");//添加
       System.out.println(c);
       c.clear();//清楚
       c.add("javaa");
       c.add("java1a");
       System.out.println(c);
       System.out.println(c.isEmpty());//验证是否非空
       System.out.println(c.size());//大小
       System.out.println(c.contains("java"));//是否包含
       c.remove("javaa");//移除
       Object[] object = c.toArray();//转化为数组
       System.out.println(object.toString());
       String [] arr  = c.toArray(new String[c.size()]);//获取指定数组
       System.out.println(arr);
    }
}
```

```
c.addAll(c2);
```

**加入别的数据，相当于拷贝**

### 遍历方式

#### 迭代器

专门遍历集合的

```
import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;
import java.util.SplittableRandom;

public class Test{
   public static void main(String[] args) {
       Collection<String> c = new ArrayList<>();//多态
       c.add("1");
       c.add("2");
       c.add("a");
      Iterator<String> it =  c.iterator();
      while (it.hasNext()){
          String ele = it.next();//指向
          System.out.println(ele);//打印
      }

    }
}
```

使用while循环来遍历，问一次取一次，交给一个变量

#### 增强for循环

```
for(数据类型 变量名：数组名或者集合){



}
```

**增强for循环时迭代器集合的简写**



#### lambda表达式

jdk8刚新增的

```
c.forEach(new Consumer<String>() {
          @Override
          public void accept(String s) {
              System.out.println(s);
          }
      });
```

foreach方法，然后使用了一个接口

最后可以简化成

```
c.forEach(System.out::println);
```

这样就能遍历了

遍历集合中的自定义对象

### List集合

List接口的常见方法：

有许多关于索引的方法

```
public class Test {
    public static void main(String[] args) {
        // 使用泛型指定 ArrayList 的元素类型为 String
        List<String> list = new ArrayList<>();
        list.add("aaa");
        list.add("bbb");
        list.add(2, "aaa"); // 在索引 2 处插入元素 "aaa"
        
        list.remove(2);//删除数据，但是会返回数据
        list.get(2);//根据索引取元素
        list.set(2,"s");//将索引设置为某个值，然后返回原来的数据
        
    }
}

```

#### 遍历方式

**for循环是可以直接使用的**（因为他有索引）

Collection的三种遍历方式也是可以继续使用的

**ArrayList和LinkedList的底层采用的数据结构是不同的**

ArrayList是基于数组实现的，根据索引查询速度比较快。但是删除效率比较低，添加效率也比较低

#### LinkedList

是基于双链表实现的

特点

- 查询慢，无论如何都要从头开始查
- 链表的增删比较快
- 双向链表的查询比单向链表的查询更快一点，双向链表在首尾进行增删改查的速度是极快的

**还有对首部和尾部操作的特定方法**

应用场景：

- 队列 一般就是先进先出，后进后出

- 栈

  栈的特点是，**后进先出，先进后出**，一端开口，一端不开

  **进栈叫压**，在后面叫栈底元素

  **出栈叫弹**，在前面叫栈顶元素

例如手枪的弹夹

简单的栈的实例

```
public class Test {
    public static void main(String[] args) {
        LinkedList<String> queue = new LinkedList<>();
        queue.addFirst("yi");
        queue.addFirst("er");
        queue.addFirst("san");
        queue.addFirst("si");
        queue.removeFirst();
        queue.removeFirst();
        System.out.println(queue);
    }
}
```

**push = addFirst**

**pop = removeFirst**

### Set集合

特点

- 添加的元素是无序的，是没有索引的，不重复的

```
import java.util.HashSet;
import java.util.Set;

public class Test {
    public static void main(String[] args) {
        Set<Integer> set = new HashSet<>();//经典代码，多态
        set.add(66);
        set.add(777);
        System.out.println(set);

    }
}
```

简单的查看其特点

无序是指的是只会无序一次，之后就都是这个顺序的了

**set使用一般都是collection的方法，没啥新增的**

#### HashSet

hash值：

就是一个Int的值，就是这个对象的代表

调用Object提供的hashcode码

不同的一般不同，相同了就是出现**哈希碰撞**

是一种增删改查比较好的

hash表jdk8之后是数组+链表+红黑树组成的

当链表长度超过8，数组长度》+64的时候，自动链表转成**红黑树**

#### LinkedHashSet

每一个元素都会额外多了一个双链表的机制，记录前后元素的位置

#### TreeSet

可排序（默认升序，按照元素的大小）

**底层是基于红黑树的**

```
import java.util.HashSet;
import java.util.Set;
import java.util.TreeSet;

public class Test {
    public static void main(String[] args) {
        Set<Integer> set = new TreeSet<>();
        set.add(1);
        set.add(3);
        set.add(77);
        System.out.println(set);

    }
}
```

**对于integer double是按照数值**

**char按照首字母编号升序**

**自定义的类型，是无法直接排序的**

自定义排序规则

- 使用Comparable接口里面的compareTo来指定比较规则

**左边对象大于右边则正整数**

- 使用有参构造器里的Comparetor对象

选择的时候选最近的对象

### 总结

记住元素的添加顺序，存储重复的元素，又要频繁的根据索引查数据

使用ArratList集合

记住元素的添加顺序，首位增删较多

使用LinkedList

不在意顺序，不要重复，增删改查快

使用Hashset

记住添加顺序，没有存储，增删改查快

使用LinkedhashSet

进行排序，增删改查快

使用TreeSet

## 集合的并发修改

使用集合的迭代器遍历的时候，又在删除，就会出现并发异常

解决方法：

```
for(int i=0;i<list.size;i++){
           String name = list.get(i);
           if(name.contains("a")){
               list.remove(name);
               i--;
           }
       }
```

 **i--;**

使用i--让他从新开始遍历，这样就不会有错误和遗漏了

或者从后往前进行遍历就可以

**迭代器的方法**

一定不要用迭代器remove（变量）方法

使用迭代器自己的remove方法

**增强for循环不能解决bug**