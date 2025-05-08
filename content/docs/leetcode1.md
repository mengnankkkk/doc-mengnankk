---
title: Leetcode二分查找专项
categories:
  - java
  
abbrlink: 2701
date: 2025.4.19
tags: 
   - java 
   - 数据结构
   - leetcode
   - 算法
---

# 二分查找

二分查找的原理就是取一个中间值，然后那中间值和目标值进行比较。

如果比目标值大的话，说明目标值在左边，中间值mid就变为右边right

相对应的，小于目标值的话，说明目标值在右边，中间值mid就变为left

二分查找的总结：

必须数组/序列是**有序的**，二分前必须先进行排序。

要确定搜索区间常见形式：`[lo, hi]`、`[lo, hi)`、`(lo, hi]`、`(lo, hi)`

确定开区间闭区间

开区间：

```java
private int lowerBound(int[] nums,int right,int target){
        int left=-1;
        while (left+1<right){
            int mid = (left+right)>>>1;
            if (nums[mid]>=target){
                right=mid;
            }else {
                left=mid;
            }
        }
        return right;
    }
```

闭区间：

```java
class Solution{
    public int searchInsert(int[] nums,int target){
        int left = 0,right = nums.length-1;
        while (left<=right){
            int mid = (left+right)/2;
            if (nums[mid] == target){
                return mid;
            }
            else if (nums[mid]<target){
                left =mid+1;
            }
            else {
                right = mid-1;
            }
        }
        return left;
    }
}
```

mid的取值：通常用 `mid = lo + (hi - lo) / 2`（或无符号右移 `>>> 1`）这样来防止溢出

还要设计check条件：

将问题转化为一个布尔函数 `check(mid)`，能准确告诉你“mid 是否满足某侧条件”。

根据 `check(mid)` 结果，把 `lo` 或 `hi` 缩到 `mid` 及其左／右一侧。

```java
int lo = L, hi = R;
while (lo < hi) {
    int mid = lo + (hi - lo) / 2;
    if (check(mid)) {
        hi = mid;      // 保留 mid
    } else {
        lo = mid + 1;  // 丢弃 mid
    }
}
return lo;

```

`lowerBound`: 找到**第一个 ≥ target** 的索引

`upperBound`: 找到**第一个 > target** 的索引

| 场景                    | 区间形式   | 备注                                           |
| ----------------------- | ---------- | ---------------------------------------------- |
| 查找某值 / 插入位置     | `[0, n-1]` | 经典闭区间；找不到时返回 `lo` 作为插入点       |
| lowerBound / upperBound | `(-1, n]`  | 开区间；`left = -1, right = n`                 |
| 最接近元素（差值比较）  | `[0, n-k]` | 窗口长度为 `k`，比较左右边界距离取决于差值大小 |
| 双指针对撞              | `lo < hi`  | 例如找最大满足条件的下标                       |

## [2563. 统计公平数对的数目](https://leetcode.cn/problems/count-the-number-of-fair-pairs/)

给你一个下标从 **0** 开始、长度为 `n` 的整数数组 `nums` ，和两个整数 `lower` 和 `upper` ，返回 **公平数对的数目** 。

如果 `(i, j)` 数对满足以下情况，则认为它是一个 **公平数对** ：

- `0 <= i < j < n`，且
- `lower <= nums[i] + nums[j] <= upper`

```java
class Solution119{
    public long countFairPairs(int[] nums,int lower, int upper){
        Arrays.sort(nums);
        long ans = 0;
        for (int j=0;j<nums.length;j++){
            int r = lowerBound(nums,j,upper-nums[j]+1);
            int l = lowerBound(nums,j,lower-nums[j]);
            ans += r-l;
        }
        return ans;
    }
    private int lowerBound(int[] nums,int right,int target){
        int left=-1;
        while (left+1<right){
            int mid = (left+right)>>>1;
            if (nums[mid]>=target){
                right=mid;
            }else {
                left=mid;
            }
        }
        return right;
    }
}
```

使用二分查找，最后target=right

`lower <= nums[i] + nums[j] <= upper`进行移项

```java
注意要在 [0, j-1] 中二分，因为题目要求两个下标 i < j
```

因为不相等的话upper那个就要+1

lower-nums[i]

upper-nums[j]+1

最后两个数量相减的和就是对数，就是答案

## [33. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/)

整数数组 `nums` 按升序排列，数组中的值 **互不相同** 。

在传递给函数之前，`nums` 在预先未知的某个下标 `k`（`0 <= k < nums.length`）上进行了 **旋转**，使数组变为 `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`（下标 **从 0 开始** 计数）。例如， `[0,1,2,4,5,6,7]` 在下标 `3` 处经旋转后可能变为 `[4,5,6,7,0,1,2]` 。

给你 **旋转后** 的数组 `nums` 和一个整数 `target` ，如果 `nums` 中存在这个目标值 `target` ，则返回它的下标，否则返回 `-1` 。

你必须设计一个时间复杂度为 `O(log n)` 的算法解决此问题

```java
class Solution{
    public int search(int[] nums,int target){
        if (nums==null||nums.length==0){
            return -1;
        }
        int start = 0;
        int end = nums.length-1;
        int mid;
        while (start<=end){
            mid = start+(end-start)/2;
            if (nums[mid] ==target){
                return mid;
            }
            if (nums[start]<=nums[mid]){
                if (target>=nums[start]&&target<nums[mid]){
                    end  = mid-1;
                }else {
                    start = mid+1;
                }
            }else {
                if (target<=nums[end]&&target>nums[mid]){
                    start = mid +1;
                }
                else {
                    end = mid-1;
                }
            }
        }
        return -1;
    }
}
```

这就是一个二分查找

if (nums[start]<=nums[mid]){
                if (target>=nums[start]&&target<nums[mid]){
                    end  = mid-1;
                }else {
                    start = mid+1;
                }
            }

如果在start小于mid的话

如果是target值在start和mid值之间的话

end = mid-1

否则的话，也就是说target值不在这

start=mid+1

else {
                if (target<=nums[end]&&target>nums[mid]){
                    start = mid +1;
                }
                else {
                    end = mid-1;
                }
            }

如果target值在mid和end之间的话

start=mid

不在的话end=mid-1

这样进行二分查找就能找到那个target值

如果这样都没找到话return-1

## [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)

给你一个按照非递减顺序排列的整数数组 `nums`，和一个目标值 `target`。请你找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 `target`，返回 `[-1, -1]`。

你必须设计并实现时间复杂度为 `O(log n)` 的算法解决此问题。

```java
class Solution{
    public int[] searchRange(int[] nums,int target){
        int start = lowBound(nums,target);
        if (start==nums.length||nums[start] !=target){
            return new int[] {-1,-1};
        }
        int end = lowBound(nums,target+1)-1;
        return new int[] {start,end};
    }
    private int lowBound(int[] nums,int target){
        int left = -1;
        int right = nums.length;
        while (left+1<right){
            int mid = left+(right-left)/2;
            if (nums[mid]>=target){
                right = mid;
            }else {
                left = mid;
            }
        }
        return right;
    }
}
```

因为是查找一个target值的目标为止，我们先设定好一个二分查找的方法

```java
private int lowBound(int[] nums,int target){
        int left = -1;
        int right = nums.length;
        while (left+1<right){
            int mid = left+(right-left)/2;
            if (nums[mid]>=target){
                right = mid;
            }else {
                left = mid;
            }
        }
        return right;
    }
```

注意的是，这里的**mid开头并不是0**

所以mid=left+(right-left)/2

while 循环的条件，如果是 left <= right，就是闭区间；如果是 left < right，就是半闭半开区间；如果是 left + 1 < right，就是开区间。这里我们选择开区间，就是不可以取到边界值的

先把目标值找出来就是start

int start = lowBound(nums,target);

如果数组里没有这个数，或者是超出范围的话，返回{-1，-1}

int end = lowBound(nums,target+1)-1;

然后找出end结束值

要想找到 ≤target 的最后一个数，无需单独再写一个二分。我们可以先找到这个数的右边相邻数字，也就是 >target 的第一个数。在所有数都是整数的前提下，>target 等价于 ≥target+1，这样就可以复用我们已经写好的二分函数了，即 lowerBound(nums, target + 1)，算出这个数的下标后，将其减一，就得到 ≤target 的最后一个数的下标。

所以最后的数组就是{start,end}

## [35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/)

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 `O(log n)` 的算法



```java
class Solution{
    public int searchInsert(int[] nums,int target){
        int left = 0,right = nums.length-1;
        while (left<=right){
            int mid = (left+right)/2;
            if (nums[mid] == target){
                return mid;
            }
            else if (nums[mid]<target){
                left =mid+1;
            }
            else {
                right = mid-1;
            }
        }
        return left;
    }
}
```

很简单的二分查找，直接开始就行

left <= right，就是闭区间，所以说边界值是可以取到的

## [704. 二分查找](https://leetcode.cn/problems/binary-search/)

给定一个 `n` 个元素有序的（升序）整型数组 `nums` 和一个目标值 `target` ，写一个函数搜索 `nums` 中的 `target`，如果目标值存在返回下标，否则返回 `-1`。

```java
class Solution704{
    public int search(int[] nums, int target) {
        int left=0,right = nums.length-1;
        while (left<=right){
            int mid = left+(right-left)/2;
            if (nums[mid]==target){
                return mid;
            }
            else if (nums[mid]<target){
                left=mid+1;
            }
            else {
                right = mid -1;
            }
        }
        return -1;
    }
}


```

简单的二分查找

注意的是

一般选择闭区间来写，这样right可以取到

right = nums.length-1,while(left<=right)

一般这样写

然后mid = left+(right-left)/2防止溢出

## [744. 寻找比目标字母大的最小字母](https://leetcode.cn/problems/find-smallest-letter-greater-than-target/)

给你一个字符数组 `letters`，该数组按**非递减顺序**排序，以及一个字符 `target`。`letters` 里**至少有两个不同**的字符。

返回 `letters` 中大于 `target` 的最小的字符。如果不存在这样的字符，则返回 `letters` 的第一个字符。

```java
class Solution744{
    public char nextGreatestLetter(char[] letters, char target){
        int l =0,r=letters.length-1;
        while (l<r){
            int mid=(l+r)>>1;
            if (letters[mid]>target) r = mid;
            else l=mid+1;
        }
        return letters[l]>target?letters[l]:letters[0];
    }
}
```

经典的二分查找，而且是闭区间的，

int mid=(l+r)>>1;这里相当于取l+r的中点，而且防止了mid的溢出

## [2529. 正整数和负整数的最大计数](https://leetcode.cn/problems/maximum-count-of-positive-integer-and-negative-integer/)

给你一个按 **非递减顺序** 排列的数组 `nums` ，返回正整数数目和负整数数目中的最大值。

- 换句话讲，如果 `nums` 中正整数的数目是 `pos` ，而负整数的数目是 `neg` ，返回 `pos` 和 `neg`二者中的最大值。

**注意：**`0` 既不是正整数也不是负整数。

```java
class Solution {
    public int maximumCount(int[] nums) {
        int neg = 0;
        int pos = 0;
        for(int x:nums){
            if(x<0){
                neg++;
            }
            else if(x>0){
                pos++;
            }
        }
        return Math.max(neg,pos);
    }
}
```

简单的递归写法

使用二分查找



```java
class Solution2529 {
    public int maximumCount(int[] nums) {
        int n = nums.length;
        int negativeCount = findFirstIndex(nums, 0); // 第一个 >= 0 的位置就是负数个数
        int positiveCount = n - findFirstIndex(nums, 1); // 第一个 > 0 的位置就是正数个数
        return Math.max(negativeCount, positiveCount);
    }

    // 返回第一个 >= target 的索引
    private int findFirstIndex(int[] nums, int target) {
        int l = 0, r = nums.length;
        while (l < r) {
            int mid = l + ((r - l) >> 1); // 防止溢出
            if (nums[mid] < target) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        return l;
    }
}


```

## [2300. 咒语和药水的成功对数](https://leetcode.cn/problems/successful-pairs-of-spells-and-potions/)

给你两个正整数数组 `spells` 和 `potions` ，长度分别为 `n` 和 `m` ，其中 `spells[i]` 表示第 `i` 个咒语的能量强度，`potions[j]` 表示第 `j` 瓶药水的能量强度。

同时给你一个整数 `success` 。一个咒语和药水的能量强度 **相乘** 如果 **大于等于** `success` ，那么它们视为一对 **成功** 的组合。

请你返回一个长度为 `n` 的整数数组 `pairs`，其中 `pairs[i]` 是能跟第 `i` 个咒语成功组合的 **药水** 数目。

```java
class Solution2300{
    public int[] successfulPairs(int[] spells, int[] potions, long success){
        Arrays.sort(potions);
        for (int i=0;i<spells.length;i++){
            long target = (success-1)/spells[i];
            if (target<potions[potions.length-1]){
                spells[i] = potions.length-upperBound(potions,(int)target);
            }else {
                spells[i]=0;
                
            }
        }
        return spells;
    }
    private int upperBound(int[] nums,int target){
        int left = -1,right = nums.length;
        while (left+1<right){
            int mid = (left+right)>>>1;
            if (nums[mid]>target){
                right = mid;
            }
            else {
                left = mid;
            }
        }
        return right;
    }
}
```

对于正整数来说：
$$
xy≥success 等价于 y≥⌈ 
x
success
​
 ⌉。
$$
为了方便二分，可以利用如下恒等式：
$$
⌈ 
b/a
​
 ⌉=⌊ 

a+b−1/b
​
 ⌋=⌊ 

a−1/b
​
 ⌋+1
$$
所以我们可以得到
$$
y>⌊ 

success−1/x
​
 ⌋
$$
对 potions 排序后，就可以二分查找了：设 x=spells[i]，j 是最小的满足 potions[j]>
(success−1)/x

j的下标，由于数组已经排序，那么下标大于 j 的也同样满足该式，这一共有 m−j 个，其中 m 是 potions 的长度。

## [1385. 两个数组间的距离值](https://leetcode.cn/problems/find-the-distance-value-between-two-arrays/)

给你两个整数数组 `arr1` ， `arr2` 和一个整数 `d` ，请你返回两个数组之间的 **距离值** 。

「**距离值**」 定义为符合此距离要求的元素数目：对于元素 `arr1[i]` ，不存在任何元素 `arr2[j]` 满足 `|arr1[i]-arr2[j]| <= d` 

对于 *arr*1 中的元素 *x*，如果 *arr*2 没有在 [*x*−*d*,*x*+*d*] 中的数，那么答案加一。

```java
class Solution1385{
    public int findTheDistanceValue(int[] arr1, int[] arr2, int d){
        Arrays.sort(arr2);
        int ans = 0;
        for (int x:arr1){
            int i  = Arrays.binarySearch(arr2,x-d);
            if (i<0){
                i=~i;
            }
            if (i==arr2.length||arr2[i]>x+d){
                ans++;
            }
        }
        return ans;
    }
}
```

直接遍历使用二分查找，查找有没有在arr2中的值

如果没有的化，i是负数，取反，找到应该插入的位置

如果不存在这个数的话，计数ans++

return ans

## [2389. 和有限的最长子序列](https://leetcode.cn/problems/longest-subsequence-with-limited-sum/)

给你一个长度为 `n` 的整数数组 `nums` ，和一个长度为 `m` 的整数数组 `queries` 。

返回一个长度为 `m` 的数组 `answer` ，其中 `answer[i]` 是 `nums` 中 元素之和小于等于 `queries[i]` 的 **子序列** 的 **最大** 长度 。

**子序列** 是由一个数组删除某些元素（也可以不删除）但不改变剩余元素顺序得到的一个数组。



```java
class Solution2389{
    private int upperBound(int[] nums,int target){
        int left = -1,right = nums.length;
        while (left+1<right){
            int mid = left+(right-left)/2;
            if (nums[mid]>target){
                right = mid;
            }
            else {
                left = mid;
            }
        }
        return right;
    }
    public int[] answerQueries(int[] nums, int[] queries){
        Arrays.sort(nums);
        for (int i =1;i<nums.length;i++){
            nums[i]+=nums[i-1];
        }
        for (int i =0;i<queries.length;i++){
            queries[i] = upperBound(nums,queries[i]);
        }
        return queries;
    }
}
```

就是先求出这个前缀和

然后再进行遍历，queries[i]赋值于二分查找能不能找到这个值

这里的等于号，**求大于用 >，求大于等于用 >=**

## [1170. 比较字符串最小字母出现频次](https://leetcode.cn/problems/compare-strings-by-frequency-of-the-smallest-character/)

定义一个函数 `f(s)`，统计 `s` 中**（按字典序比较）最小字母的出现频次** ，其中 `s` 是一个非空字符串。

例如，若 `s = "dcce"`，那么 `f(s) = 2`，因为字典序最小字母是 `"c"`，它出现了 2 次。

现在，给你两个字符串数组待查表 `queries` 和词汇表 `words` 。对于每次查询 `queries[i]` ，需统计 `words` 中满足 `f(queries[i])` < `f(W)` 的 **词的数目** ，`W` 表示词汇表 `words` 中的每个词。

请你返回一个整数数组 `answer` 作为答案，其中每个 `answer[i]` 是第 `i` 次查询的结果。

```java
class Solution1170{
    public int[] numSmallerByFrequency(String[] queries, String[] words){
        int n = words.length;
        int[] nums = new int[n];
        for (int i =0;i<n;++i){
            nums[i] = f(words[i]);
        }
        Arrays.sort(nums);
        int m  = queries.length;
        int[] ans = new int[m];
        for (int i =0;i<m;++i){
            int x = f(queries[i]);
            int l=0,r= n;
            while (l<r){
                int mid = (l+r)>>1;
                if(nums[mid]>x){
                    r = mid;
                }
                else {
                    l = mid+1;
                }
            }
            ans[i]=n-l;
        }
        return ans;

    }
    private int f(String s){
        int[] cnt = new int[26];
        for (int i =0;i<s.length();++i){
            ++cnt[s.charAt(i)-'a'];
        }
        for (int x:cnt){
            if (x>0){
                return x;
            }
        }
        return 0;
    }
}
```

首先先把f(x)函数写出来，就是统计words的字典序比较最小字母的出现频次

然后把f(w)计算出来，存入数组nums,进行排序

然后进行二分查找，进行比较，找到在 ***nums*** 中二分查找第一个大于 *f*(q) 的位置 *i*

然后后面的就都满足f(q)<f(w)，所以数量就是n-i

把n-i存入ans[i]中，然后返回ans即可

## [2080. 区间内查询数字的频率](https://leetcode.cn/problems/range-frequency-queries/)

请你设计一个数据结构，它能求出给定子数组内一个给定值的 **频率** 。

子数组中一个值的 **频率** 指的是这个子数组中这个值的出现次数。

请你实现 `RangeFreqQuery` 类：

- `RangeFreqQuery(int[] arr)` 用下标从 **0** 开始的整数数组 `arr` 构造一个类的实例。
- `int query(int left, int right, int value)` 返回子数组 `arr[left...right]` 中 `value` 的 **频率** 。

一个 **子数组** 指的是数组中一段连续的元素。`arr[left...right]` 指的是 `nums` 中包含下标 `left` 和 `right` **在内** 的中间一段连续元素。

```java
class RangeFreQuery{
    private int lowBound(List<Integer> a,int target){
        int left = -1,right = a.size();
        while (left+1<right){
            int mid = (left+right)>>>1;
            if (a.get(mid)<target){
                left = mid;
            }else {
                right = mid;
            }
        }
        return right;
    }
    private final Map<Integer, List<Integer>> pos = new HashMap<>();
    public RangeFreQuery(int[] arr){
        for (int i=0;i<arr.length;i++){
            pos.computeIfAbsent(arr[i],k->new ArrayList<>()).add(i);
        }

    }
    public int query(int left,int right,int value){
        List<Integer> a = pos.get(value);
        if (a==null){
            return 0;
        }
        return lowBound(a,right+1)-lowBound(a,left);
    }

}
```

## [3488. 距离最小相等元素查询](https://leetcode.cn/problems/closest-equal-element-queries/)

给你一个 **循环** 数组 `nums` 和一个数组 `queries` 。

对于每个查询 `i` ，你需要找到以下内容：

- 数组 `nums` 中下标 `queries[i]` 处的元素与 **任意** 其他下标 `j`（满足 `nums[j] == nums[queries[i]]`）之间的 **最小** 距离。如果不存在这样的下标 `j`，则该查询的结果为 `-1` 。

返回一个数组 `answer`，其大小与 `queries` 相同，其中 `answer[i]` 表示查询`i`的结果。

```java
class Solution3488{
    public List<Integer> solveQueries(int[] nums, int[] queries){
        Map<Integer,List<Integer>> indices = new HashMap<>();
        for (int i =0;i<nums.length;i++){
            indices.computeIfAbsent(nums[i],k->new ArrayList<>()).add(i);
        }//构建hash表，indices存储

        int n = nums.length;
        for (List<Integer> p :indices.values()){
            int i0 = p.get(0);
            p.add(0,p.get(p.size()-1)-n);//循环向左的哨兵
            p.add(i0+n);//循环向右的哨兵
        }
        List<Integer> ans  = new ArrayList<>(queries.length);
        for (int i :queries){
            List<Integer> p = indices.get(nums[i]);
            if (p.size()==3){
                ans.add(-1);//没有，只有一次
            }else {
                int j = Collections.binarySearch(p,i);//二分查找位置，i在p的位置
                ans.add(Math.min(i-p.get(j-1),p.get(j+1)-i));//比较前一个和后一个
            }
        }
        return ans;
    }
}
```

## [2563. 统计公平数对的数目](https://leetcode.cn/problems/count-the-number-of-fair-pairs/)

给你一个下标从 **0** 开始、长度为 `n` 的整数数组 `nums` ，和两个整数 `lower` 和 `upper` ，返回 **公平数对的数目** 。

如果 `(i, j)` 数对满足以下情况，则认为它是一个 **公平数对** ：

- `0 <= i < j < n`，且
- `lower <= nums[i] + nums[j] <= upper`

```java
class Solution{
    public long countFairPairs(int[] nums,int lower, int upper){
        Arrays.sort(nums);
        long ans = 0;
        for (int j=0;j<nums.length;j++){
            int r = lowerBound(nums,j,upper-nums[j]+1);//右边的边界
            int l = lowerBound(nums,j,lower-nums[j]);//左边的边界
            ans += r-l;
        }
        return ans;
    }
    private int lowerBound(int[] nums,int right,int target){
        int left=-1;
        while (left+1<right){
            int mid = (left+right)>>>1;
            if (nums[mid]>=target){
                right=mid;
            }else {
                left=mid;
            }
        }
        return right;
    }
}
```

[2070. 每一个查询的最大美丽值](https://leetcode.cn/problems/most-beautiful-item-for-each-query/)

给你一个二维整数数组 `items` ，其中 `items[i] = [pricei, beautyi]` 分别表示每一个物品的 **价格** 和 **美丽值** 。

同时给你一个下标从 **0** 开始的整数数组 `queries` 。对于每个查询 `queries[j]` ，你想求出价格小于等于 `queries[j]` 的物品中，**最大的美丽值** 是多少。如果不存在符合条件的物品，那么查询的结果为 `0` 。

请你返回一个长度与 `queries` 相同的数组 `answer`，其中 `answer[j]`是第 `j` 个查询的答案。



**二分查找的时候一般都要将数组进行排序**

```java
class Solution2070{
    private int upperBound(int[][] items, int target){
        int left = -1;
        int right = items.length;
        while (left+1<right){
            int mid = (left+right)>>>1;
            if (items[mid][0]>target){
                    right =mid;
            }
            else{
                left = mid;
            }
        }
        return right;
    }
    public int[] maximumBeauty(int[][] items, int[] queries){
        Arrays.sort(items,(a,b)->a[0]-b[0]);//排序规则
        for (int i =1;i<items.length;i++){
            items[i][1] = Math.max(items[i][1],items[i-1][1]);//更新美丽值，是前一个位置的最大美丽值
        }
        for (int i =0;i<queries.length;i++){
            int j = upperBound(items,queries[i]);
            queries[i] =j>0?items[j-1][1]:0;//查找到了，就是前一个的最大美丽值，不是的话就是0
        }
        return queries;
    }
}
```

## [1146. 快照数组](https://leetcode.cn/problems/snapshot-array/)

实现支持下列接口的「快照数组」- SnapshotArray：

- `SnapshotArray(int length)` - 初始化一个与指定长度相等的 类数组 的数据结构。**初始时，每个元素都等于** **0**。
- `void set(index, val)` - 会将指定索引 `index` 处的元素设置为 `val`。
- `int snap()` - 获取该数组的快照，并返回快照的编号 `snap_id`（快照号是调用 `snap()` 的总次数减去 `1`）。
- `int get(index, snap_id)` - 根据指定的 `snap_id` 选择快照，并返回该快照指定索引 `index` 的值。

```
public void set(int index, int val) {
    history.computeIfAbsent(index, k -> new ArrayList<>())
           .add(new int[]{curSnapId, val});
}

```

`computeIfAbsent` 方法：如果 `history` 里**没有**当前 `index`，就**自动新建**一个空的 `ArrayList<int[]>`；如果有，就直接用已有的列表。

然后 `.add(new int[]{curSnapId, val})`：表示把当前的**快照 ID 和对应的值**作为一个数组 `{curSnapId, val}` 加到列表中。

```java
class SnapshotArray{
    private int curSnapId;
    private final Map<Integer,List<int[]>> history = new HashMap<>();

    public SnapshotArray(int length){

    }
    public void set(int index,int val){
        history.computeIfAbsent(index,k->new ArrayList<>()).add(new int[]{curSnapId,val});
    }
    public int snap(){
        return curSnapId++;
    }
    public int get(int index,int snapId){
        if (!history.containsKey(index)){
            return 0;
        }
        List<int[]> h = history.get(index);
        int j  = search(h,snapId);
        return j<0?0:h.get(j)[1];
    }
    private int search(List<int[]> h,int x){
        int left = -1;
        int right = h.size();
        while (left+1<right){
            int mid = (left+right)>>>1;
            if (h.get(mid)[0]<=x){
                left = mid;
            }else {
                right = mid;
            }
        }
        return left;
    }
}
```

注意数组h.get(mid)[0]是指的curSnapId

跟snapId相对应了

## [981. 基于时间的键值存储](https://leetcode.cn/problems/time-based-key-value-store/)

设计一个基于时间的键值数据结构，该结构可以在不同时间戳存储对应同一个键的多个值，并针对特定时间戳检索键对应的值。

实现 `TimeMap` 类：

- `TimeMap()` 初始化数据结构对象
- `void set(String key, String value, int timestamp)` 存储给定时间戳 `timestamp` 时的键 `key` 和值 `value`。
- `String get(String key, int timestamp)` 返回一个值，该值在之前调用了 `set`，其中 `timestamp_prev <= timestamp` 。如果有多个这样的值，它将返回与最大  `timestamp_prev` 关联的值。如果没有值，则返回空字符串（`""`）。

```java
class TimeMap {
    private final Map<String, List<Info>> tmap;

    static class Info {
        String value;
        int timestamp;

        public Info(String value, int timestamp){
            this.value = value;
            this.timestamp = timestamp;
        }
    }

    public TimeMap() {
        tmap = new HashMap<>();
    }

    public void set(String key, String value, int timestamp) {
        tmap.computeIfAbsent(key,k -> new ArrayList<>()).add(new Info(value, timestamp));
    }

    public String get(String key, int timestamp) {
        if (!tmap.containsKey(key)){
            return "";
        }
        List<Info> tmp = tmap.get(key);
        int left = -1, right = tmp.size();
        while (left + 1 < right){
            int mid = (left + right) >>> 1;
            if (tmp.get(mid).timestamp > timestamp){
                right = mid;
            } else {
                left = mid;
            }
        }
        return left < 0 ? "" : tmp.get(left).value;
    }
}

```

跟1146一样

## [658. 找到 K 个最接近的元素](https://leetcode.cn/problems/find-k-closest-elements/)

给定一个 **排序好** 的数组 `arr` ，两个整数 `k` 和 `x` ，从数组中找到最靠近 `x`（两数之差最小）的 `k` 个数。返回的结果必须要是按升序排好的。

整数 `a` 比整数 `b` 更接近 `x` 需要满足：

- `|a - x| < |b - x|` 或者
- `|a - x| == |b - x|` 且 `a < b`

```java
class Solution658{
    public List<Integer> findClosestElements(int[] arr, int k, int x){
        List<Integer> list = new ArrayList<>();//存放数据
        int n = arr.length;
        int left = 0;
        int right  = n-k;
        while (left<=right){
            int mid = (left+right)>>>1;
            if (mid+k<n&&x-arr[mid]>arr[mid+k]-x){
                left= mid+1;
            }else {
                right = mid-1;
            }
        }
        for (int i =left;i<left+k;i++){//找到<=x
            list.add(arr[i]);
        }
        return list;
    }
}
```

if (mid+k<n&&x-arr[mid]>arr[mid+k]-x){
                left= mid+1;
    }else {
                right = mid-1;
}

mid+k<n保证不越界

然后

x-arr[mid]>arr[mid+k]-x说明还是左边的距离更大，应该往右移动

所以mid = left+1

直到找到<=x的那个点，加入list之中

## [1287. 有序数组中出现次数超过25%的元素](https://leetcode.cn/problems/element-appearing-more-than-25-in-sorted-array/)

给你一个非递减的 **有序** 整数数组，已知这个数组中恰好有一个整数，它的出现次数超过数组元素总数的 25%。

请你找到并返回这个整数

```java
class Solution1287{
    public int findSpecialInteger(int[] arr){
        int n  = arr.length;
        int l  = 0,r = n/4;
        while (r<n){
            if (arr[l]==arr[r]) return arr[r];
            l++;
            r++;
        }
        return -1;
    }
}
```

因为如果某个数字的出现次数超过了 25%，那么**在它第一次出现的位置**和**它向右移动 `n/4` 的位置**，**一定也还是它本身**。

当l=r的时候，就是这个数了

## [2071. 你可以安排的最多任务数目](https://leetcode.cn/problems/maximum-number-of-tasks-you-can-assign/)

给你 `n` 个任务和 `m` 个工人。每个任务需要一定的力量值才能完成，需要的力量值保存在下标从 **0** 开始的整数数组 `tasks` 中，第 `i` 个任务需要 `tasks[i]` 的力量才能完成。每个工人的力量值保存在下标从 **0** 开始的整数数组 `workers` 中，第 `j` 个工人的力量值为 `workers[j]` 。每个工人只能完成 **一个** 任务，且力量值需要 **大于等于** 该任务的力量要求值（即 `workers[j] >= tasks[i]` ）。

除此以外，你还有 `pills` 个神奇药丸，可以给 **一个工人的力量值** 增加 `strength` 。你可以决定给哪些工人使用药丸，但每个工人 **最多** 只能使用 **一片** 药丸。

给你下标从 **0** 开始的整数数组`tasks` 和 `workers` 以及两个整数 `pills` 和 `strength` ，请你返回 **最多** 有多少个任务可以被完成。



整体的思路是使用二分查找这个数组

然后检查是不是能够完成

然后根据条件来移动left，right

然后因为是最多，也就是找到最小的那个，二分查找就是返回left

最难的是check方面

能不能完成工作

这个时候就要使用到队列，taks完成就poll出去

这个时候就要用到贪心算法，让最强的k个工人去完成最简单的k个任务



```java
class Solution2071{
    public int maxTaskAssign(int[] tasks, int[] workers, int pills, int strength){
        Arrays.sort(tasks);
        Arrays.sort(workers);

        int left  = 0;
        int right = Math.min(tasks.length,workers.length)+1;
        while (left+1<right){
            int mid = (left+right)>>>1;
            if (check(tasks,workers,pills,strength,mid)){
                left = mid;
            }else {
                right = mid;
            }

        }
        return left;

    }
    private boolean check(int[] tasks, int[] workers, int pills, int strength, int k){
        Deque<Integer> validTasks = new ArrayDeque<>();
        int i =0;
        for (int j =workers.length-k;j<workers.length;j++){
            int w = workers[j];
            while (i<k&&tasks[i]<=w+strength){
                validTasks.add(tasks[i]);
                i++;

            }
            if (validTasks.isEmpty()){
                return false;
            }
            if(w>=validTasks.peekFirst()){
                validTasks.pollFirst();
            }else{
                if (pills==0) return false;
                pills--;
                validTasks.pollLast();
            }

        }
        return true;
    }
}
```

### [1234. 替换子串得到平衡字符串](https://leetcode.cn/problems/replace-the-substring-for-balanced-string/)

有一个只含有 `'Q', 'W', 'E', 'R'` 四种字符，且长度为 `n` 的字符串。

假如在该字符串中，这四个字符都恰好出现 `n/4` 次，那么它就是一个「平衡字符串」。

 

给你一个这样的字符串 `s`，请通过「替换一个子串」的方式，使原字符串 `s` 变成一个「平衡字符串」。

你可以用和「待替换子串」长度相同的 **任何** 其他字符串来完成替换。

请返回待替换子串的最小可能长度。

如果原字符串自身就是一个平衡字符串，则返回 `0`。

```java
class Solution{
    public int balancedString(String S){
        char[] s= S.toCharArray();
        int[] cnt = new int['X'];
        for (char c:s){
            cnt[c]++;
        }
        int n = s.length;
        int m =n/4;
        if (cnt['Q'] == m && cnt['W'] == m && cnt['E'] == m && cnt['R'] == m) {
            return 0; // 已经符合要求啦
        }
        int ans  = n;
        int left = 0;
        for (int right = 0;right<n;right++){
            cnt[s[right]]--;
            while (cnt['Q'] <= m && cnt['W'] <= m && cnt['E'] <= m && cnt['R'] <= m){
                ans = Math.min(ans,right-left+1);
                cnt[s[left]]++;
                left++;
            }
        }
        return ans;
    }
}
```

如果在待替换子串**之外**的任意字符的出现次数都不超过 *m*，那么可以通过替换，使 *s* 为平衡字符串，即每个字符的出现次数均为 *m*。

然后使用滑动窗口解决
