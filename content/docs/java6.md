---
title: javase jdk8新特性
categories:
  - javase
abbrlink: 2701
date: 2024.8.14
tags: 
   - java
---

# jdk8新特性

## Stream

新出的一个特性，也叫Steram流

**用来操作集合和数组的数据**

可以使代码更简介，可读性更强

不使用：

```
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Test {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>();
        Collections.addAll(names, "aaa", "abb", "cccc");  // 修正了语法错误，使用逗号分隔元素

        List<String> list = new ArrayList<>();
        for (String name : names) {
            if(name.startsWith("a") && name.length() == 3){
                list.add(name);
            }
        }
        System.out.println(list);
    }
}

```

使用Stream流

```
import javax.swing.plaf.synth.SynthUI;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.stream.Collectors;

public class Test {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>();
        Collections.addAll(names, "aaa", "abb", "cccc");  // 修正了语法错误，使用逗号分隔元素

      List<String> list =   names.stream().filter(s -> s.startsWith("a")).filter//filter方法用来过滤
                (s -> s.length()== 3).collect(Collectors.toList());//collect用来收集
      System.out.println(list);
    }
}

```

是一个流水线

可以用来排序去重等等一系列内容

最后对处理的数据进行收集使用

**主要学的是中间的方法**

集合调用：

```
Collection;
        Stream<String> Stream = names.stream();
Set:
        Stream<String> stream = names.stream();
Map:
        Set<String> keys = map.keySet();
        Stream<String> ks = keys.stream();
        Set<String> ks = map.values();
        Stream<String> v = value.stream();
```

或者是把map中的键值对看作一个整体，调用entrySet方法然后再获取Stream流

数组调用：

```
String [] qwqs = {"aa","bbb"};
        Stream<String> s1 = Arrays.stream(qwqs);
        Stream<String> s2 = Stream.of(qwqs);
```

调用的两种方法

### 常见方法

**最核心的**

#### 中间方法

```
names.stream().filter(s -> s>=60).sorted().forEach(s -> System.out.println(s));
```

foreach()方法是无返回值的，被称为终结方法，可以用来遍历

```
sorted((o1, o2) -> o2.length() - o1.length())
```

降序排序

```
limit(3)
```

限制输出的个数

```
skip(names.size()-2)
```

跳过多少个方法

```
names.stream().map(s -> s.getBytes()).distinct();
```

map方法用来加工，也可以叫映射，distinct用来去重。

用来去重，要重写hashcode和equals方法重写才能真正去重

```
import java.util.stream.Collectors;
import java.util.stream.Stream;

public static void main(String[] args) {
        Stream<String> s1 = Stream.of("aa", "aaa");
        Stream<String> s2 = Stream.of("aaaaa", "aaaaaaa");
        Stream<String> all = Stream.concat(s1, s2);

        // 将流中的元素收集到一个字符串中
        String result = all.collect(Collectors.joining(", "));
        System.out.println(result);
}
```

连接方法和收集方法

#### 终结方法



```
names.stream().filter(s -> s>=60).sorted().forEach(s -> System.out.println(s));
```

foreach()方法是无返回值的，被称为终结方法，可以用来遍历

```
count();
```

count()方法用来统计数量

```
names.stream().max((o1, o2) -> ).get();
```

获取最高值，里面是比较规则

最小值同理

#### 收集Stream流

把最后的结果收集到集合或者数组中去

```
List<String> list =   names.stream().filter(s -> s.startsWith("a")).filter//filter方法用来过滤
                (s -> s.length()== 3).collect(Collectors.toList());//collect用来收集
```

收集到LIst集合中去

**流只能收集一次**，**不能重复的**

Map集合收集的时候，要声明谁是值和键

```
names.stream().collect(Collectors.toMap(a ->a.getBytes(),a -> a.length()));
```

把流中的数据接到数组中去

```
String [] a = new String[0] names.stream().toArray();
        System.out.println(Arrays.toString(a));
```



## Lambda