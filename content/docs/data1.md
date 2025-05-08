---
title: 数据结构 时间复杂度+线性表栈篇
categories:
  - 408
abbrlink: 2701
date: 2024.8.3
tags: 

   - 数据结构 

---



# 介绍

数据结构：计算机存储组织数据的方式，相互搭建一种或多种特定关系的数据元素的集合。在内存中将数据存储起来。

算法：定义良好的计算的过程

********十分重要，要学扎实********

## 时间复杂度

评估算法的好坏，评估他的空间和时间。分别是时间复杂度和空间复杂度

现在关注时间复杂度更多

时间复杂度是一个函数（数学中带有未知数的函数表达式），算法中基本操作的执行次数，为算法的时间复杂度

环境不同，执行的时间就会不同。

### 嵌套循环的时间复杂度

时间复杂度的函数式①：

F(N)=N*N+2*N+10

把上面的式子简化，只留影响最大的一项。**n越大，后两项对结果的影响就越小。**算个大概就行了

时间复杂度不需要精确的数值

用了O的渐进表示法，保留影响最大的就行了，就是一个大概的值

所以时间复杂度O（N^2）

O的渐进表示法：

- 如果有常数就用1去替代
- 只保留最高阶项
- 最高阶项存在但不是1，则去除与这个项目相乘的常数，得到就是大O阶

**一般情况下时间复杂度计算时，未知数都是用的N，别的也是可以的**

根据条件，可将O继续缩小

常数：

O（1）是指的能够运算常数次，不是仅仅代表算法只能运行一次

### strchar

```
while(*str){
if(*str == character)
return str;
else 
++str;
}


```

用来查找一个字符串的函数

这个算法是有变化的，查找的东西不同，其时间复杂度就会不同

分为三种情况

- 最好 下界
- 最坏 上界
- 平均 期望

**一个算法随着输入不同，时间复杂度不同，时间复杂度就会做悲观预期，就是看最坏的情况：**

O（N）

计算的太复杂，可能会看平均值

### 冒泡排序

精确的冒泡排序时间复杂度：

**算法不能只看代码，要看过程**

F(n) = n*(N-1)/2=N^2

### 二分查找

最好的情况O（1）

最坏情况O（）查找次数是：**查找log以2为底N的对数**

在n个数中一共查找log以2为底N的对数次

## 空间复杂度

**S(n)复杂度是常数，可以称算法可以原地工作**

内存中存储的有：

- 程序代码（大小是固定的）

- 数据（一些变量啥的）就占用的内存相加

  最后也是O()的计算方式，只关心他的阶数

for循环的调用，每一次占新的一层内存空间。

递归调用带来的内存开销，空间复杂度=递归调用的深度

# 线性结构和非线性结构

线性结构：是一个有序数据元素的集合

一对一的线性关系

常见的

- 线性表
- 数组
- 栈
- 队列

非线性结构

常见

- 树
- 图
- 二维数组
- 多维数组



# 线性表

定义：**各个数据元素之间的关系**

L=(a1,22.....an)**数据类型相同**，占用的空间一样大。是一个有限的序列,n为表长。n=0时为空表

第几个成为位序，位序时从1开始的

表头表尾

除开第一个元素（头节点）外，每个元素有且仅有一个直接**前驱**

除开最后一个元素（尾节点）外，每个元素有且仅有一个直接**后继**

数据元素中是**一对一**的关系

## 基本操作

初始化：构造一个空的，分配内存空间

销毁表：销毁操作，释放线性表所占的内存空间

插入：ListInsert

删除:ListDelete

按值查找：LocateElem

按位查找：GetElem

Length:返回长度

PrintList：打印

Empty：是非为空

**主要是增删改查的操作**

**名字要具有可读性才可以**

参数的修改结果要带回来用&，改成引用类型

可以能是没带回来的

## 顺序表

用数组的顺序存储的方式实现的线性表

**逻辑上相邻，物理上也相邻**

静态分配：

用一个内存空间来存储数组（如果数组满了，就放弃这个数组吧）

在打印的时候，会出现脏数据（之前在这一快内存中的数据）

用基本操作来处理更好

存储的元素都是连续的

顺序表的api的实例

```
public class Test<T> {
    private T[] eles; // 数组
    private int N;    // 元素数量

    // 构造方法，初始化容量
    public Test(int capacity) {
        this.eles = (T[]) new Object[capacity];
        this.N = 0;
    }

    // 清空线性表
    public void clear() {
        this.N = 0;
    }

    // 判断线性表是否为空
    public boolean isEmpty() {
        return this.N == 0;
    }

    // 返回线性表长度
    public int length() {
        return this.N;
    }

    // 获取指定位置的元素
    public T get(int i) {
        if (i < 0 || i >= N) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }
        return eles[i];
    }

    // 在末尾插入元素
    public void insert(T t) {
        if (N == eles.length) {
            resize(eles.length * 2); // 动态扩容
        }
        eles[N++] = t;
    }

    // 在指定位置插入元素
    public void insert(int i, T t) {
        if (i < 0 || i > N) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }
        if (N == eles.length) {
            resize(eles.length * 2); // 动态扩容
        }
        // 将 i 及以后的元素后移
        for (int index = N; index > i; index--) {
            eles[index] = eles[index - 1];
        }
        eles[i] = t; // 插入元素
        N++;
    }

    // 移除指定位置的元素
    public T remove(int i) {
        if (i < 0 || i >= N) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }
        T current = eles[i];
        // 将 i 后面的元素前移
        for (int index = i; index < N - 1; index++) {
            eles[index] = eles[index + 1];
        }
        eles[--N] = null; // 避免对象游离
        if (N > 0 && N == eles.length / 4) {
            resize(eles.length / 2); // 动态缩小
        }
        return current;
    }

    // 返回指定元素的索引，找不到返回 -1
    public int index(T t) {
        for (int i = 0; i < N; i++) {
            if (eles[i].equals(t)) {
                return i;
            }
        }
        return -1;
    }

    // 动态调整数组大小
    private void resize(int newSize) {
        T[] temp = (T[]) new Object[newSize];
        for (int i = 0; i < N; i++) {
            temp[i] = eles[i];
        }
        eles = temp;
    }
}

```

方法使用实现的

```
import java.util.Arrays;

public class Test<T> {
    private T[] eles; // 数组
    private int N;    // 元素数量

    // 构造方法，初始化容量
    public Test(int capacity) {
        this.eles = (T[]) new Object[capacity];
        this.N = 0;
    }

    // 清空线性表
    public void clear() {
        this.N = 0;
    }

    // 判断线性表是否为空
    public boolean isEmpty() {
        return this.N == 0;
    }

    // 返回线性表长度
    public int length() {
        return this.N;
    }

    // 获取指定位置的元素
    public T get(int i) {
        if (i < 0 || i >= N) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }
        return eles[i];
    }

    // 在末尾插入元素
    public void insert(T t) {
        if (N == eles.length) {
            resize(eles.length * 2); // 动态扩容
        }
        eles[N++] = t;
    }

    // 在指定位置插入元素
    public void insert(int i, T t) {
        if (i < 0 || i > N) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }
        if (N == eles.length) {
            resize(eles.length * 2); // 动态扩容
        }
        // 将 i 及以后的元素后移
        for (int index = N; index > i; index--) {
            eles[index] = eles[index - 1];
        }
        eles[i] = t; // 插入元素
        N++;
    }

    // 移除指定位置的元素
    public T remove(int i) {
        if (i < 0 || i >= N) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }
        T current = eles[i];
        // 将 i 后面的元素前移
        for (int index = i; index < N - 1; index++) {
            eles[index] = eles[index + 1];
        }
        eles[--N] = null; // 避免对象游离
        if (N > 0 && N == eles.length / 4) {
            resize(eles.length / 2); // 动态缩小
        }
        return current;
    }

    // 返回指定元素的索引，找不到返回 -1
    public int index(T t) {
        for (int i = 0; i < N; i++) {
            if (eles[i].equals(t)) {
                return i;
            }
        }
        return -1;
    }

    // 动态调整数组大小
    private void resize(int newSize) {
        T[] temp = (T[]) new Object[newSize];
        for (int i = 0; i < N; i++) {
            temp[i] = eles[i];
        }
        eles = temp;
    }

    public static void main(String[] args) {
        Test<String> list = new Test<>(5);
        list.insert("qwq");
        list.insert("qwqwq");
        System.out.println(list);
    }

    @Override
    public String toString() {
        return "Test{" +
                "eles=" + Arrays.toString(eles) +
                ", N=" + N +
                '}';
    }
}

```

容量的扩容实现，其实就是一个动态的扩展的方法

```
private void resize(int newSize) {
        T[] temp = (T[]) new Object[newSize];
        for (int i = 0; i < N; i++) {
            temp[i] = eles[i];
        }
        eles = temp;
```

然后是需要使用的时候直接调用这个方法就可以

## 链式表

**逻辑上相邻，物理上并不是相邻的**

是通过**指针**将数据联系起来的

存储的元素不一定是连续的

********元素节点存放的数据元素（date)以及相邻元素的地址信息(next)********

LinkedList就是基于链表实现的

更改数据快，但是查询比较慢

```
import org.w3c.dom.Node;

public class BigStar<T> {
    public T item;
    public Node next;
    public Node(T item,Node next){
        this.item = item;
        this.next = next;

    }

}
```

单项链表，就是刚刚的情况，有date区域和next区域

链表的实现（加入了迭代器之后的）

```
import java.util.Iterator;
import java.util.NoSuchElementException;

public class LinkedList<T> implements Iterable<T> {
    // 头节点，链表的起点
    private Node<T> head;
    // 链表中的元素数量
    private int N;

    // 链表构造函数，初始化链表
    public LinkedList() {
        this.head = null;
        this.N = 0;
    }

    // 清空链表
    public void clear() {
        this.head = null;
        this.N = 0;
    }

    // 判断链表是否为空
    public boolean isEmpty() {
        return this.N == 0;
    }

    // 获取链表长度
    public int length() {
        return this.N;
    }

    // 获取指定位置的元素
    public T get(int i) {
        if (i < 0 || i >= N) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }
        Node<T> current = head;
        for (int index = 0; index < i; index++) {
            current = current.next;
        }
        return current.item;
    }

    // 在链表末尾插入元素
    public void insert(T t) {
        if (head == null) {
            head = new Node<>(t, null);
        } else {
            Node<T> current = head;
            while (current.next != null) {
                current = current.next;
            }
            current.next = new Node<>(t, null);
        }
        N++;
    }

    // 在指定位置插入元素
    public void insert(int i, T t) {
        if (i < 0 || i > N) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }
        if (i == 0) {
            head = new Node<>(t, head);
        } else {
            Node<T> current = head;
            for (int index = 0; index < i - 1; index++) {
                current = current.next;
            }
            current.next = new Node<>(t, current.next);
        }
        N++;
    }

    // 删除指定位置的元素
    public T remove(int i) {
        if (i < 0 || i >= N) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }
        T removedItem;
        if (i == 0) {
            removedItem = head.item;
            head = head.next;
        } else {
            Node<T> current = head;
            for (int index = 0; index < i - 1; index++) {
                current = current.next;
            }
            removedItem = current.next.item;
            current.next = current.next.next;
        }
        N--;
        return removedItem;
    }

    // 查找元素的索引，若不存在则返回 -1
    public int index(T t) {
        Node<T> current = head;
        for (int i = 0; i < N; i++) {
            if (current.item.equals(t)) {
                return i;
            }
            current = current.next;
        }
        return -1;
    }

    // 内部节点类
    private static class Node<T> {
        T item;      // 节点数据
        Node<T> next; // 下一个节点引用

        // 构造函数
        public Node(T item, Node<T> next) {
            this.item = item;
            this.next = next;
        }
    }

    // 实现 Iterable 接口的 iterator 方法，返回一个链表的迭代器
    @Override
    public Iterator<T> iterator() {
        return new LinkedListIterator();
    }

    // 内部迭代器类，实现 Iterator 接口
    private class LinkedListIterator implements Iterator<T> {
        private Node<T> current = head;

        // 判断是否还有下一个元素
        @Override
        public boolean hasNext() {
            return current != null;
        }

        // 获取下一个元素
        @Override
        public T next() {
            if (!hasNext()) {
                throw new NoSuchElementException();
            }
            T item = current.item;
            current = current.next;
            return item;
        }

        // 移除元素操作，在链表中不常用，可以不实现
        @Override
        public void remove() {
            throw new UnsupportedOperationException();
        }
    }
}

```

链式表的另一种api实现

```
            }
            newNode.next = current.next;
            current.next = newNode;
        }
        size++;
    }

    public T remove(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }
        Node<T> removedNode;
        if (index == 0) {
            removedNode = head;
            head = head.next;
        } else {
            Node<T> current = head;
            for (int i = 0; i < index - 1; i++) {
                current = current.next;
            }
            removedNode = current.next;
            current.next = removedNode.next;
        }
        size--;
        return removedNode.data;
    }

    public T get(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }
        Node<T> current = head;
        for (int i = 0; i < index; i++) {
            current = current.next;
        }
        return current.data;
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public int size() {
        return size;
    }

    public void clear() {
        head = null;
        size = 0;
    }

    public void printList() {
        Node<T> current = head;
        while (current != null) {
            System.out.print(current.data + " ");
            current = current.next;
        }
        System.out.println();
    }

    public static void main(String[] args) {
        SimLinked<Integer> list = new SimLinked<>();
        list.add(1);
        list.add(2);
        list.add(3);
        list.printList();  // 输出：1 2 3

        list.add(1, 4);
        list.printList();  // 输出：1 4 2 3

        list.remove(2);
        list.printList();  // 输出：1 4 3

        System.out.println( list.get(1));  // 输出：4
        System.out.println(list.size());  // 输出：3
    }
}

```

双向链表的实现

```
public class LinkedList<T> {
    // Node class for the doubly linked list
    private static class Node<T> {
        T data;
        Node<T> next;
        Node<T> prev;

        Node(T data) {
            this.data = data;
            this.next = null;
            this.prev = null;
        }
    }

    private Node<T> head;
    private Node<T> tail;
    private int size;

    // Constructor
    public LinkedList() {
        this.head = null;
        this.tail = null;
        this.size = 0;
    }

    // Add element at the end of the list
    public void add(T data) {
        Node<T> newNode = new Node<>(data);
        if (head == null) {
            head = newNode;
            tail = newNode;
        } else {
            tail.next = newNode;
            newNode.prev = tail;
            tail = newNode;
        }
        size++;
    }

    // Add element at a specific index
    public void add(int index, T data) {
        if (index < 0 || index > size) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }
        Node<T> newNode = new Node<>(data);
        if (index == 0) {
            if (head == null) {
                head = newNode;
                tail = newNode;
            } else {
                newNode.next = head;
                head.prev = newNode;
                head = newNode;
            }
        } else if (index == size) {
            tail.next = newNode;
            newNode.prev = tail;
            tail = newNode;
        } else {
            Node<T> current = head;
            for (int i = 0; i < index; i++) {
                current = current.next;
            }
            newNode.next = current;
            newNode.prev = current.prev;
            current.prev.next = newNode;
            current.prev = newNode;
        }
        size++;
    }

    // Remove element at a specific index
    public T remove(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }
        Node<T> removedNode;
        if (index == 0) {
            removedNode = head;
            if (head == tail) {
                head = null;
                tail = null;
            } else {
                head = head.next;
                head.prev = null;
            }
        } else if (index == size - 1) {
            removedNode = tail;
            tail = tail.prev;
            tail.next = null;
        } else {
            Node<T> current = head;
            for (int i = 0; i < index; i++) {
                current = current.next;
            }
            removedNode = current;
            current.prev.next = current.next;
            current.next.prev = current.prev;
        }
        size--;
        return removedNode.data;
    }

    // Get element at a specific index
    public T get(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }
        Node<T> current = head;
        for (int i = 0; i < index; i++) {
            current = current.next;
        }
        return current.data;
    }

    // Check if the list is empty
    public boolean isEmpty() {
        return size == 0;
    }

    // Get the size of the list
    public int size() {
        return size;
    }

    // Clear the list
    public void clear() {
        head = null;
        tail = null;
        size = 0;
    }

    // Print the elements of the list in forward order
    public void printListForward() {
        Node<T> current = head;
        while (current != null) {
            System.out.print(current.data + " ");
            current = current.next;
        }
        System.out.println();
    }

    // Print the elements of the list in reverse order
    public void printListBackward() {
        Node<T> current = tail;
        while (current != null) {
            System.out.print(current.data + " ");
            current = current.prev;
        }
        System.out.println();
    }
}

```

链表的反转：

单向链表的反转，头不能反转。反转的是后面的元素

头指向尾，然后再继续反转

加入了反转功能的链表，及其封装使用

```
public class LinkedList<T> {
    private static class Node<T>{
        T data;
        Node<T> prev;
        Node<T> next;

        Node(T data){
            this.data = data;
        }
    }
    private Node<T> head;
    private Node<T> tail;
    private int size;

    public LinkedList() {
        this.head = null;
        this.tail = null;
        this.size = 0;
    }
    public void addFirst(T data){
        Node<T> newNode = new Node<>(data);
        if (head == null) {
            head = newNode;
            tail = newNode;
        } else {
            newNode.next = head;
            head.prev = newNode;
            head = newNode;
        }
        size++;
    }
    public T removeFirst(){
        if (head ==null){
            throw new IllegalStateException("有错误");
        }
        Node<T> removedNode = head;
        if (head ==tail){
            head = null;
            tail = null;
        }else {
            head = head.next;
            head.prev = null;
        }
        size--;
        return removedNode.data;
    }
    public T removeLast(){
        if (head ==null){
            throw new IllegalStateException("有错误");
        }
        Node<T> removedNode = head;
        if (head==tail){
            head = null;
            tail = null;
        }else {
            tail = tail.prev;
            tail.next = null;
        }
        size--;
        return removedNode.data;
    }
    public boolean contains(T data){
        Node<T> current = head;
        while (current != null){
            if (current.data.equals(data)){
                return true;
            }
            current = current.next;
        }
        return false;
    }
    public boolean remove(T data){
        Node<T> current = head;
        while (current != null){
            if (current.data.equals(data)){
                if (current == head){
                    removeFirst();
                } else if (current == tail) {
                    removeLast();
                }else {
                    current.prev.next = current.next;
                    current.next.prev = current.prev;
                    size--;
                }
                return true;
            }
            current = current.next;
        }
        return false;
    }
    public void reverse(){
        Node<T> current = head;
        Node<T> temp = null;
        while (current != null){
            temp = current.prev;
            current.prev = current.next;
            current.next = temp;
            current = current.prev;
        }
        if (temp!=null){
            head = temp.prev;
        }
    }
    public int getSize(){
        return size;
    }
    public boolean isEmpty(){
        return size ==0;
    }
    public void printForward(){
        Node<T> current = head;
        while (current != null){
            System.out.println(current.data+" ");
            current = current.next;
        }
        System.out.println();
    }
    public void printBackward(){
        Node<T> current = tail;
        while (tail!=null){
            System.out.println(current.data+" ");
            current = current.prev;
        }
        System.out.println();
    }
}
```

封装使用

```
public class Main {
    public static void main(String[] args) {
        DoublyLinkedList<Integer> list = new DoublyLinkedList<>();

        // 添加元素到链表
        list.addFirst(10);
        list.addLast(20);
        list.addLast(30);
        list.addFirst(5);

        // 打印链表内容
        list.printForward();  // 输出: 5 10 20 30
        list.printBackward(); // 输出: 30 20 10 5

        // 反转链表
        list.reverse();
        list.printForward();  // 输出: 30 20 10 5

        // 查找和删除元素
        System.out.println("Contains 20? " + list.contains(20));  // 输出: true
        list.remove(20);
        list.printForward();  // 输出: 30 10 5

        // 获取链表长度
        System.out.println("Size: " + list.size());  // 输出: 3

        // 删除头尾元素
        list.removeFirst();
        list.removeLast();
        list.printForward();  // 输出: 10

        // 检查链表是否为空
        System.out.println("Is empty? " + list.isEmpty());  // 输出: false

        // 清空链表
        list.removeFirst();
        System.out.println("Is empty after remove all? " + list.isEmpty());  // 输出: true
    }
}

```

解析反转功能：

```
public void reverse(){
        Node<T> current = head;
        Node<T> temp = null;
        while (current != null){
            temp = current.prev;
            current.prev = current.next;
            current.next = temp;
            current = current.prev;
        }
        if (temp!=null){
            head = temp.prev;
        }
    }
```

首先先设置了头节点和暂时的中间节点

然后

```
temp = current.prev;
current.prev = current.next;
current.next = temp;
```

向当与current的前驱和后继进行交换

然后current指向现在的前驱，即一开始的朝向尾部的位置

然后temp不为空的时候，头节点移动到current前驱的前驱的位置

重复操作即可得到反转的链表

# 栈

限制插入和删除只能在**第一个位置**上进行的线性表

是一个开口封底的

操作有push(压栈)和pop（出栈）

末端叫栈顶，头部叫栈底

**先进后出，后进先出**

使用链表方式实现的叫动态栈

使用数组方式实现叫静态栈

动态栈可以自动调节大小

```
import java.util.Arrays;

public class Test {
    private int maxStack;
    private int[] stack;
    private int top = -1;
    public ArrayStack(int maxStack){
        this.maxStack = maxStack;
    }
    public boolean isfull(){
        this.top = this.maxStack -1;
    }
    public  boolean isempty(){
        return this.top == -1;
    }
    public void push(int val){
        if(isfull()){
            throw new RuntimeException("满栈");
        }
        top++;
        stack[top] = val;
    }
    public int pop(){
        if(isempty()){
            throw  new RuntimeException("空栈");
        }
        int value = stack[top];
        top--;
        return value;
    }
    public void list(){
        if (isempty()){
            throw new RuntimeException("空");
        }
        for (int i =0;i<stack.length;i++){
            System.out.println(stack[i]);
        }
    }
}
```

数组模拟栈的使用

```
public class Stack<T> {
    private T[] stackArray;
    private int maxSize;
    private int top;

    public Stack(int size){
        this.maxSize = size;
        this.stackArray = (T[]) new Object[maxSize];//强转类型
        this.top= -1;
    }
    public void push(T value){
        if (isFull()){
            throw new StackOverflowError("full!");
        }
        stackArray[++top] = value;
    }
    public T pop(){
        if (isEmpty()){
            throw new IllegalStateException("empty!");
        }
        return stackArray[top--];
    }
    public T peek(){
        if (isEmpty()){
            throw new IllegalStateException("empty!");
            
        }
        return stackArray[top];
    }
    public boolean isEmpty() {
        return top == -1;  // 如果栈顶指针为 -1，则栈为空
    }
    public boolean isFull() {
        return top == maxSize - 1;
    }
    public int size(){
        return top  + 1;
    }



```

或者是用java中的集合进行模拟栈的操作

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

用栈来实现计算器的操作

```
import java.util.Stack;

public class Calculator {
    private static boolean isOperator(char c) {
        return c == '+' || c == '-' || c == '*' || c == '/';
    }

    private static int precedence(char op) {
        if (op == '+' || op == '-') return 1;
        if (op == '*' || op == '/') return 2;
        return 0;
    }

    // 这里增加了一个操作符参数
    private static int applyOperation(int a, int b, char op) {
        switch (op) {
            case '+': return a + b;
            case '-': return a - b;
            case '*': return a * b;
            case '/': return a / b;
            default: return 0;
        }
    }

    public static int evaluate(String expression) {
        Stack<Integer> values = new Stack<>();
        Stack<Character> ops = new Stack<>();

        for (int i = 0; i < expression.length(); i++) {
            char c = expression.charAt(i);

            if (c == ' ') continue;

            // 如果是数字，处理数字部分
            if (Character.isDigit(c)) {
                StringBuilder sb = new StringBuilder();
                while (i < expression.length() && Character.isDigit(expression.charAt(i))) {
                    sb.append(expression.charAt(i++));
                }
                values.push(Integer.parseInt(sb.toString()));
                i--; // 调整索引位置
            }
            // 左括号直接入栈
            else if (c == '(') {
                ops.push(c);
            }
            // 右括号处理，直到遇到左括号
            else if (c == ')') {
                while (ops.peek() != '(') {
                    int b = values.pop();
                    int a = values.pop();
                    values.push(applyOperation(a, b, ops.pop()));
                }
                ops.pop(); // 去掉左括号
            }
            // 如果是操作符
            else if (isOperator(c)) {
                while (!ops.isEmpty() && precedence(ops.peek()) >= precedence(c)) {
                    int b = values.pop();
                    int a = values.pop();
                    values.push(applyOperation(a, b, ops.pop()));
                }
                ops.push(c);
            }
        }

        // 处理剩余的操作符
        while (!ops.isEmpty()) {
            int b = values.pop();
            int a = values.pop();
            values.push(applyOperation(a, b, ops.pop()));
        }

        return values.pop();
    }

    public static void main(String[] args) {
        String expression = "10 + 2 * 6";  // Example exprsion
        System.out.println("Result: " + evaluate(expression));
    }
}


```

递归是数据结构经常用的方法，通过不同情况下的情况，进行分类，实现递归。递归要有**递归的方法**和**结束的条件**

## 栈堆

使用堆栈可以很方便的保存和取用信息，因此长被用作算法和程序中的辅助存储结构，临时保存信息，供后面操作中使用。

```
import java.util.Stack;

public class Souluntion {
    public boolean isValid(String s) {
        // 如果字符串长度是奇数，直接返回false
        if (s.length() % 2 == 1) {
            return false;
        }

        Stack<Character> stack = new Stack<>();

        // 遍历字符串中的每个字符
        for (char ch : s.toCharArray()) {
            // 如果是左括号，入栈
            if (ch == '(' || ch == '[' || ch == '{') {
                stack.push(ch);
            } else if (ch == ')') {
                // 如果栈非空且栈顶是相应的左括号，弹栈
                if (!stack.isEmpty() && stack.peek() == '(') {
                    stack.pop();
                } else {
                    return false;
                }
            } else if (ch == ']') {
                if (!stack.isEmpty() && stack.peek() == '[') {
                    stack.pop();
                } else {
                    return false;
                }
            } else if (ch == '}') {
                if (!stack.isEmpty() && stack.peek() == '{') {
                    stack.pop();
                } else {
                    return false;
                }
            }
        }
        // 如果栈为空，说明所有括号匹配，返回true，否则false
        return stack.isEmpty();
    }
}

```

