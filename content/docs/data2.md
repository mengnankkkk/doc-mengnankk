---
title: 数据结构 排序算法
categories:
  - 408
abbrlink: 2701
date: 2024.8.15
tags: 

   - 数据结构 
---

# 排序算法

这些开发的时候基本用不到，但是笔试的时候用的比较高

是一组数据按照指定的顺序进行排列

java提供了一个接口**Comparable**来定义排序规则

```

public class Test  implements Comparable<Test>{
    private Integer stu_id;
    private Integer stu_age;
    public Student(Integer stu_id,Integer stu_age){
        this.stu_id = stu_id;
        this.stu_age = stu_age;
    }
    public Student(){
    }

    @Override
    public int compareTo(Test o) {
        return this.stu_age - o.stu_age;
    }
}
```

- 大于0则前面大

- 等于0则一样大

- 小于0则后面大

## 基数排序

分配式排序

**将比较的数值统一设置为相同的位数，比较小的前面补0**

步骤

- 首先确定最大（最长）的数
- 在不足的前面补0
- 从最低位开始排序，创建10个桶0—9，按照顺序放入对应的桶中
- 把之前数据按照顺序放在一个临时的桶中。然后把桶清空
- 然后再开始排下一位的数
- 然后再次重复

案例：

```java
import java.util.ArrayList;

public class Test {
    private static final int[] array = {82,50,66,48,21,79,14,37,25};
    public static int[] sort(int[] array){
        int max = 0;
        for(int temp :array){
            if(temp>max){
                max = temp;
            }
        }
        int maxDigit = 0;
        while (max != 0){
            max /=10;
            maxDigit++;
        }
        ArrayList<ArrayList<Integer>> bucket = new ArrayList<>();
        for(int i=0;i<10;i++){
            bucket.add(new ArrayList<Integer>());
        }
        int moid = 10;
        int div = 1;
        for(int i = 0;i<maxDigit;i++,moid *=10,div*=10){//每次迭代乘以10
            for (int j = 0;j<array.length;j++){
               int num = (array[j] % moid)/div;
               bucket.get(num).add(array[j]);
            }
            int index = 0;
            for (int k = 0;k<bucket.size();k++){
                ArrayList<Integer> list = bucket.get(k);
                for(int m =0;m<list.size();m++){
                    array[index++] = list.get(m);
                }
                bucket.get(k).clear();
            }
        }
        return array;
    }
}
```

## 冒泡排序

从前往后以此比较相邻元素的值，发现逆序则交换，使得较大的元素主键往后移

一轮确定一个最大值（相对来说的）

```
import java.util.Arrays;

public class Test {
    public static void main(String[] args) {
        int[] arrays = {4, 10, 6, 7, 8};
        int[] sort = sort(arrays);
        System.out.println(Arrays.toString(sort));
    }
    public static int [] sort(int[] arrays){
        int temp =0;
        for(int i = 0;i<arrays.length-1;i++){
            for(int j = 0;j<arrays.length-1-i;j++){
                if(arrays[j]>arrays[j+1]){
                    temp = arrays[j];
                    arrays[j] = arrays[j+1];
                    arrays[j+1] = temp;
                }
            }
        }
        return arrays;
    }
}
```

********冒泡排序方法实现********

## 快速排序

是冒泡排序的一种改进

首先会对数据进行分割，然后层层分割

整个排序过程可以递归进行

步骤

- 首先先选定一个基准元素，然后头上选一个指针，尾部选一个指针
- 和基准元素进行比较，保证左边小，右边大。不符合则交换位置
- 如果上面的不符合的话，直到找到不符合的，然后两个指针进行交换
- 如果两个指针重合，则和基准元素交换位置
- 交换完了指针继续移动，以此进行
- 最后指针重合之后，按照基准元素分割，再按照上面的选基准元素

代码实现

```
import java.util.Arrays;

public class Test {
    public static void main(String[] args) {
        int[] arrays = new int[]{1, 4, 6, 2, 8};
        quick(arrays, 0, arrays.length - 1);
        System.out.println(Arrays.toString(arrays));
    }

    public static void quick(int[] arr, int startIndex, int endIndex) {
        // 基准条件检查，避免递归栈溢出
        if (startIndex >= endIndex) {
            return;
        }
        // 获取分区点
        int pIndex = partition(arr, startIndex, endIndex);
        // 递归排序左子数组
        quick(arr, startIndex, pIndex - 1);
        // 递归排序右子数组
        quick(arr, pIndex + 1, endIndex);
    }

    public static int partition(int[] arr, int startIndex, int endIndex) {
        int pivot = arr[startIndex]; // 选择第一个元素作为基准
        int left = startIndex;       // 左指针
        int right = endIndex;        // 右指针

        while (left < right) {
            // 从右向左找到第一个小于pivot的元素
            while (left < right && arr[right] >= pivot) {
                right--;
            }
            // 从左向右找到第一个大于pivot的元素
            while (left < right && arr[left] <= pivot) {
                left++;
            }
            // 交换左右指针所指的元素
            if (left < right) {
                int temp = arr[left];
                arr[left] = arr[right];
                arr[right] = temp;
            }
        }

        // 将pivot放到正确的位置
        arr[startIndex] = arr[left];
        arr[left] = pivot;

        // 返回基准点的索引
        return left;
    }
}

```

