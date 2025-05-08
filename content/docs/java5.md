---
title: javase 集合 Map和其他相关
categories:
  - javase
abbrlink: 2701
date: 2024.8.11
tags: 
   - java
---

# 其他相关

## 可变参数

一种特殊的形参

```
数据类型...参数名称
```

可以不传参数，也可以传多个或者一个或者数组

```
public class Test {
    public static void main(String[] args) {
       test();
    }
    public static void test(int...nums){

    }
}
```

**常常用来接受数据**

**可变参数在方法内部是一个数组**

注意事项：

- 一个形参列表中，只能有一个可变参数
- 必须在形参列表的最后面

## 工具类

Collections是一个用来操作集合的工具类

```
List<String> names = new ArrayList<>();
Collections.addAll(names,"与马仔","我累哦二","array");//批量添加数据
Collections.shuffle(names);//打乱List的里面数据，在斗地主中可以进行洗牌的操作
Collections.sort(names);//对List里面的进行升序排序,自定义的类和前面一样
```

几个常用的方法

# Map集合

**Map集合是双列集合，一次要存一对数据**

**前面的叫做键，后面的叫做值。被成为键值对**

**所有的键是不能够重复的，但是值可以重复**

**键和值是一一对应的**

Map是一个泛型接口



常见类型：

- Hashmap 无序不重复无索引
- Treemap 有序，不重复，无索引
- LinkedHashMap 按照大小默认升序，不重复无索引

常用方法：

常见的方法都有。

get(Object key)获取键值

```
import java.rmi.MarshalledObject;
import java.util.*;

public class Test {
    public static void main(String[] args) {
        Map<String ,Integer> map = new HashMap<>();
        map.put("a",5);
        map.put("d",4);
        map.put("e",7);
        map.put("b",6);
        System.out.println(map.get("e"));//根据键获取对应的值map
        System.out.println(map.remove("a"));//根据键删除，然后返回值
        System.out.println(map.containsKey("a"));//判断是否有某个键
        //类似的还有是否包含某个值，注意值的类型
        Set<String> keys = map.keySet();
        System.out.println(keys);//将Map集合里的全部值放到Set集合里
        //类似的还有值
        Map<String,Integer> map1 = new HashMap<>();
        Map<String,Integer> map2 = new HashMap<>();
        map1.putAll(map2);//将map2里的元素都倒入一份到map1里面
    }

}
```



## 遍历方式

键找值：

先获取键然后再找值

键值对：看成一个整体来找

Lambda

### 键找值

```
Set<String > keys =  map.keySet();//用Set集合来存储键
for(String key:keys){//增强for循环
  int value =  map.get(key);//根据键获取对应的值
  System.out.println(value);
}
```

### 键值对

```
Set<Map.Entry<String, Integer>> entries = map.entrySet();//设置Entry对象
for (Map.Entry<String, Integer> entry : entries) {//增强for循环遍历
   String key =entry.getKey();
   int value = entry.getValue();//存储值
   System.out.println(key+value);
}
```

### Lambda(最简单的)

```
map.forEach((k,v) -> {
    System.out.println(k+"---"+ v);
});
```

**直接遍历**

是java书写的一个标准，以后尽量这样写。主要是**简单**

案例

```java
import java.util.*;

public class Test {
    public static void main(String[] args) {
        List<String > date = new ArrayList<>();//新建一个List类用来放
        String [] select = {"a","b","c","d"};
        Random r = new Random();//新建一个随机类
        for(int i = 1;i<=80;i++){
           int index =  r.nextInt(4);//随机0123
           date.add(select[index]);//把随机的结果添加进去date
        }
        Map<String,Integer> result = new HashMap<>();
        for (String s : date) {//增强for循环
            if(result.containsKey(s)){//如果包含
                result.put(s,result.get(s)+1);//加一
            }
            else {
                result.put(s,1);//不包含，返回1
            }
        }
        System.out.println(result);//输出
    }
}
```

注解：

**在java中需要存储一一对应的数据的时候，就可以使用Map集合来做**

## HashMap

```
public static void main(String[] args) {
        Map<String ,Integer> map = new HashMap<>();
        map.put("a",5);
        map.put("d",4);
        map.put("c",7);
        map.put("b",6);
        System.out.println(map);

    }
```

用的最多。

底层原理：

基于hash表实现的，set集合的底层就是map集合改过来的，只不过set只要键不要值而已

哈希表=数组+链表+红黑树

增删较好的，依赖hashcode和equals方法来保持键的唯一

## LinkedHashMap

LinkHashMap是有序的

也是基于hash表实现的，有序是根据双列表实现的

LinkedHashSet就是根据LinkedHashMap实现的

## TreeMap

按照键的大小默认升序排序

基于红黑树实现的

TreeSet是根据TreeMap实现的

自定义对象的排序是跟前面一样的

# 集合的嵌套

使用案例：

```
public class Test {
    public static void main(String[] args) {
        Map<String, List<String>> map = new HashMap<>();
        List<String> cties1 = new ArrayList<>();
        Collections.addAll(cties1, "南京", "南通");
        map.put("江苏省", cties1);
    }
}
```

