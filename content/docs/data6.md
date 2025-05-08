---
title: 查找算法
categories:
  - 408
abbrlink: 2701
date: 2024.11.11
tags: 

   - 数据结构
   - 算法
---

查找算法主要用于在集合（如数组、列表、树等）中寻找特定的元素。以下是几种常见的查找算法以及相应的代码示例。

# 线性查找

线性查找是一种简单的查找方法，它逐个检查每个元素，直到找到目标元素或遍历完整个集合。

```
public class LinearSearch {
    public static int linearSearch(int[] arr, int target) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == target) {
                return i;  // 返回目标元素的索引
            }
        }
        return -1;  // 如果找不到，返回-1
    }

    public static void main(String[] args) {
        int[] arr = {2, 5, 8, 3, 6, 9};
        int target = 3;
        int result = linearSearch(arr, target);
        if (result == -1) {
            System.out.println("元素未找到");
        } else {
            System.out.println("元素 " + target + " 在索引 " + result + " 处");
        }
    }
}
```

# **二分查找**

二分查找是对已经排序的数组进行查找的有效算法。每次通过中间元素来缩小查找范围，直到找到目标元素。

```
public class BinarySearch {
    public static int binarysearch(int[] arr,int target){
        int left = 0;
        int right = arr.length - 1;
        while (left<=right){
            int mid = left + (right - left)/2;
            if (arr[mid] == target){
                return  mid;
            }
            if (arr[mid]<target){
                left = mid + 1;
            }
            else {
                right = mid - 1;
            }
        }
        return -1;
    }
    public static void main(String[] args) {
        int[] arr = {1, 3, 5, 7, 9, 11};
        int target = 7;
        int result = binarysearch(arr, target);
        if (result == -1) {
            System.out.println("元素未找到");
        } else {
            System.out.println("元素 " + target + " 在索引 " + result + " 处");
        }
    }
}

```

# 哈希查找

哈希查找通过使用哈希表来进行查找，时间复杂度通常为 O(1)，是查找速度最快的方法之一。

```
import java.util.HashSet;

public class HashSearch {
    public static boolean hashSearch(HashSet<Integer> set,int target){
        return set.contains(target);
    }

    public static void main(String[] args) {
        HashSet<Integer> set = new HashSet<>();
        set.add(1);
        set.add(5);
        set.add(3);
        set.add(6);
        set.add(7);
        int target  = 3;
        if (hashSearch(set,target)){
            System.out.println(target+"yes");
        }
        else {
            System.out.println(target+"no");
        }
    }
}

```

hash查找主要是看该数据的hash值，hash值一般是不相同的，所以能通过hash值来进行查找

# dfs

是一种遍历或查找图、树的算法，常用于图形和树形结构的查找。它通过尽可能深入地访问每个分支来遍历整个结构。

主要是用递归来解决

例如：

```
import java.util.*;

public class DFS {
    static class TreeNode {
        int value;
        TreeNode left, right;

        TreeNode(int value) {
            this.value = value;
            this.left = null;
            this.right = null;
        }
    }

    public static boolean dfs(TreeNode node, int target) {
        if (node == null) {
            return false;
        }
        if (node.value == target) {
            return true;
        }

        // 递归地搜索左子树和右子树
        return dfs(node.left, target) || dfs(node.right, target);
    }

    public static void main(String[] args) {
        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2);
        root.right = new TreeNode(3);
        root.left.left = new TreeNode(4);
        root.left.right = new TreeNode(5);

        int target = 4;
        if (dfs(root, target)) {
            System.out.println("找到了目标元素 " + target);
        } else {
            System.out.println("未找到目标元素 " + target);
        }
    }
}

```

# bfs

用于查找图或树结构中的元素。与深度优先搜索不同，广度优先搜索逐层扫描，首先访问每一层的节点。

```
import java.util.*;

public class BFS {
    static class TreeNode {
        int value;
        TreeNode left, right;

        TreeNode(int value) {
            this.value = value;
            this.left = null;
            this.right = null;
        }
    }

    public static boolean bfs(TreeNode root, int target) {
        if (root == null) {
            return false;
        }

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if (node.value == target) {
                return true;
            }

            if (node.left != null) {
                queue.offer(node.left);
            }
            if (node.right != null) {
                queue.offer(node.right);
            }
        }

        return false;
    }

    public static void main(String[] args) {
        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2);
        root.right = new TreeNode(3);
        root.left.left = new TreeNode(4);
        root.left.right = new TreeNode(5);

        int target = 5;
        if (bfs(root, target)) {
            System.out.println("找到了目标元素 " + target);
        } else {
            System.out.println("未找到目标元素 " + target);
        }
    }
}

```

# 插入查找

```
public class InterpolationSearch {
    public static int interpolationSearch(int[] arr,int target){
        int left = 0,right = arr.length - 1;
        while (left<=right&&target>=arr[left]&&target<=arr[right]){
            if (arr[right] == arr[left]){
                break;
            }
            int mid = left + ((target - arr[left]*(right-left))/(arr[right]-arr[left]));
            if(arr[mid] == target){
                return mid;
            }
            if (arr[mid]<target){
                left = mid+1;
            }
            else {
                right = mid - 1;
            }
        }
        return -1;
    }
    public static void main(String[] args) {
        int[] arr = {10, 20, 30, 40, 50, 60, 70, 80, 90, 100};
        int target = 70;
        int result = interpolationSearch(arr, target);
        if (result == -1) {
            System.out.println("元素未找到");
        } else {
            System.out.println("元素 " + target + " 在索引 " + result + " 处");
        }
    }
}


```

代码实现

# 选择排序

每次在未排序部分选择最小（或最大）元素，然后与未排序部分的第一个元素交换，直到整个数组有序。

每次只进行一次交换操作。

时间复杂度：O(n^2)，无论数据是否已经有序。

代码实现：

```
public class SelectionSort {

    public static void selectionSort(int[] arr) {
        int comparisons = 0;
        for (int i = 0; i < arr.length - 1; i++) {
            int minIndex = i;
            for (int j = i + 1; j < arr.length; j++) {
                comparisons++;
                if (arr[j] < arr[minIndex]) {
                    minIndex = j;
                }
            }

            int temp = arr[i];
            arr[i] = arr[minIndex];
            arr[minIndex] = temp;
        }
        System.out.println("！：" + comparisons);
    }

    public static void main(String[] args) {
        int[] arr = {5, 2, 9, 1, 5, 6};
        selectionSort(arr);
        for (int num : arr) {
            System.out.print(num + " ");
        }
    }
}
```

# 起泡排序

每次比较相邻的两个元素，如果顺序错误则交换它们，经过一轮比较后，最大（或最小）元素会“浮”到数组的末端。重复这个过程，直到数组有序。

其实可以也叫做冒泡排序

时间复杂度：O(n^2)，如果数据已经有序，可以优化为 O(n)。

代码实现：

```
public class BubbleSort {
    public static void bubbleSort(int[] arr){
        int comparisons =0;
        for (int i =0;i<arr.length-1;i++){
            for ( int j = 0;j<arr.length-1-i;j++){
                comparisons++;
                if (arr[j]>arr[j+1]){
                    int temp =  arr[j];
                    arr[j] = arr[j+1];
                    arr[j+1] = temp;
                }
            }
        }
        System.out.println(comparisons);
    }
}
```

