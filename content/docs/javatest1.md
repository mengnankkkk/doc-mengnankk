---
title: JAVA 练习暑期篇
categories:
  - java
  
abbrlink: 2701
date: 2024.7.28
tags: 
   - java 
   - 算法

---

# java编程练习

## 基础题

### 1.leetcode [ 回文数](https://leetcode.cn/problems/palindrome-number/)

给你一个整数 `x` ，如果 `x` 是一个回文整数，返回 `true` ；否则，返回 `false` 。



回文数

是指正序（从左向右）和倒序（从右向左）读都是一样的整数。



- 例如，`121` 是回文，而 `123` 不是。

 

**示例 1：**

```
输入：x = 121
输出：true
```

**示例 2：**

```
输入：x = -121
输出：false
解释：从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```

**示例 3：**

```
输入：x = 10
输出：false
解释：从右向左读, 为 01 。因此它不是一个回文数。
```

 

**提示：**

- `-231 <= x <= 231 - 1`

题解：

```
class Solution {
    public boolean isPalindrome(int x) {
        if(x<0) return false;
        int t = x;
        int y = 0;
        while(t>0){
            y = y * 10 + t % 10;
            t /= 10;
        }
        return x == y;

    }
}
```

**循环条件**：`while(t > 0)` 表示只要 `t` 大于 `0`，就继续循环。即，只要还有未处理的数字，就继续反转。

**反转步骤**：

- **取个位数字**：`t % 10` 取 `t` 的最后一位数字。例如，如果 `t` 是 `1234`，`t % 10` 会得到 `4`。
- **构建反转后的数字**：`y = y * 10 + t % 10` 将当前的 `y` 向左移动一位（乘以 `10`），然后加上取出的最后一位数字。这一步的效果是把 `t` 的最后一位数字移到 `y` 的末尾。例如，如果 `y` 是 `123`，`t % 10` 是 `4`，那么 `y = y * 10 + t % 10` 就会变成 `1234`。
- **移除已处理的位**：`t /= 10` 将 `t` 除以 `10`，即移除 `t` 的最后一位。例如，如果 `t` 是 `1234`，`t /= 10` 会把 `t` 变成 `123`。

### 2.leetcode [罗马数字转整数](https://leetcode.cn/problems/roman-to-integer/)

罗马数字包含以下七种字符: `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 `2` 写做 `II` ，即为两个并列的 1 。`12` 写做 `XII` ，即为 `X` + `II` 。 `27` 写做 `XXVII`, 即为 `XX` + `V` + `II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

- `I` 可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
- `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 
- `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

给定一个罗马数字，将其转换成整数。

 

**示例 1:**

```
输入: s = "III"
输出: 3
```

**示例 2:**

```
输入: s = "IV"
输出: 4
```

**示例 3:**

```
输入: s = "IX"
输出: 9
```

**示例 4:**

```
输入: s = "LVIII"
输出: 58
解释: L = 50, V= 5, III = 3.
```

**示例 5:**

```
输入: s = "MCMXCIV"
输出: 1994
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```

 

**提示：**

- `1 <= s.length <= 15`
- `s` 仅含字符 `('I', 'V', 'X', 'L', 'C', 'D', 'M')`
- 题目数据保证 `s` 是一个有效的罗马数字，且表示整数在范围 `[1, 3999]` 内
- 题目所给测试用例皆符合罗马数字书写规则，不会出现跨位等情况。
- IL 和 IM 这样的例子并不符合题目要求，49 应该写作 XLIX，999 应该写作 CMXCIX 。
- 关于罗马数字的详尽书写规则，可以参考 [罗马数字 - 百度百科](https://baike.baidu.com/item/罗马数字/772296)。

题解：

```
import java.util.*;

class Solution {
    public int romanToInt(String s) {
        int sum = 0;
        int preNum = getValue(s.charAt(0));
        for(int i = 1;i < s.length(); i ++) {
            int num = getValue(s.charAt(i));
            if(preNum < num) {
                sum -= preNum;
            } else {
                sum += preNum;
            }
            preNum = num;
        }
        sum += preNum;
        return sum;
    }
    
    private int getValue(char ch) {
        switch(ch) {
            case 'I': return 1;
            case 'V': return 5;
            case 'X': return 10;
            case 'L': return 50;
            case 'C': return 100;
            case 'D': return 500;
            case 'M': return 1000;
            default: return 0;
        }
    }
}

```

### 3.[最长公共前缀](https://leetcode.cn/problems/longest-common-prefix/)

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

 

**示例 1：**

```
输入：strs = ["flower","flow","flight"]
输出："fl"
```

**示例 2：**

```
输入：strs = ["dog","racecar","car"]
输出：""
解释：输入不存在公共前缀。
```

 

**提示：**

- `1 <= strs.length <= 200`
- `0 <= strs[i].length <= 200`
- `strs[i]` 仅由小写英文字母组成

题解：

```java
public class Solution1 {
    // 方法：找到字符串数组中最长的公共前缀
    public String longestCommonPrefix(String[] strs) {
        // 如果输入数组为空，直接返回空字符串
        if (strs.length == 0)
            return ""; // 没有字符的空情况，其字符的长度为0

        // 初始化前缀字符串为第一个字符串
        String ans = strs[0];

        // 从第二个字符串开始遍历数组
        for (int i = 1; i < strs.length; i++) {
            int j = 0;
            // 在当前字符串和前缀字符串中逐字符比较，直到遇到不同字符或者到达其中一个字符串的末尾
            for (; j < ans.length() && j < strs[i].length(); j++) {
                if (ans.charAt(j) != strs[i].charAt(j))
                    break;
            }
            // 更新前缀字符串为当前比较结果的前缀部分
            ans = ans.substring(0, j);
            // 如果前缀字符串为空，直接返回空字符串
            if (ans.equals(""))
                return ans;
        }
        // 返回最终的最长公共前缀
        return ans;
    }

    // 主方法，用于测试和验证
    public static void main(String[] args) {
        Solution1 solution = new Solution1();
        // 示例字符串数组
        String[] strs = {"flower", "flow", "flight"};
        // 输出最长公共前缀
        System.out.println("Longest common prefix: " + solution.longestCommonPrefix(strs));
    }
}

```

### 4.leetcode[删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)

给你一个 **非严格递增排列** 的数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使每个元素 **只出现一次** ，返回删除后数组的新长度。元素的 **相对顺序** 应该保持 **一致** 。然后返回 `nums` 中唯一元素的个数。

考虑 `nums` 的唯一元素的数量为 `k` ，你需要做以下事情确保你的题解可以被通过：

- 更改数组 `nums` ，使 `nums` 的前 `k` 个元素包含唯一元素，并按照它们最初在 `nums` 中出现的顺序排列。`nums` 的其余元素与 `nums` 的大小不重要。
- 返回 `k` 。

**判题标准:**

系统会用下面的代码来测试你的题解:

```
int[] nums = [...]; // 输入数组
int[] expectedNums = [...]; // 长度正确的期望答案

int k = removeDuplicates(nums); // 调用

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有断言都通过，那么您的题解将被 **通过**。

 

**示例 1：**

```
输入：nums = [1,1,2]
输出：2, nums = [1,2,_]
解释：函数应该返回新的长度 2 ，并且原数组 nums 的前两个元素被修改为 1, 2 。不需要考虑数组中超出新长度后面的元素。
```

**示例 2：**

```
输入：nums = [0,0,1,1,1,2,2,3,3,4]
输出：5, nums = [0,1,2,3,4]
解释：函数应该返回新的长度 5 ， 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4 。不需要考虑数组中超出新长度后面的元素。
```

 

**提示：**

- `1 <= nums.length <= 3 * 104`
- `-104 <= nums[i] <= 104`
- `nums` 已按 **非严格递增** 排列

题解：

```java
class Soultion{
    public int removeDuplicates (int[] nums){
        int left = 0;
        for(int i = 1;i<nums.length;i++){//用for循环解析所有字符
            if(nums[i]>nums[i-1]){//如果后比前大，则说明是一个不重复的字符
                nums[++left] = nums[i];//这里 ++left 是先自增 left，然后赋值，确保 left 指向的是数组中下一个可以放置新元素的位置。
            }
        }
        return ++left;//返回 ++left，因为 left 的初始值是 0，所以返回值是去除重复元素后的数组长度。
    }
}
```

### 5.leetcode [移除元素](https://leetcode.cn/problems/remove-element/)

给你一个数组 `nums` 和一个值 `val`，你需要 **[原地](https://baike.baidu.com/item/原地算法)** 移除所有数值等于 `val` 的元素。元素的顺序可能发生改变。然后返回 `nums` 中与 `val` 不同的元素的数量。

假设 `nums` 中不等于 `val` 的元素数量为 `k`，要通过此题，您需要执行以下操作：

- 更改 `nums` 数组，使 `nums` 的前 `k` 个元素包含不等于 `val` 的元素。`nums` 的其余元素和 `nums` 的大小并不重要。
- 返回 `k`。

**用户评测：**

评测机将使用以下代码测试您的解决方案：

```
int[] nums = [...]; // 输入数组
int val = ...; // 要移除的值
int[] expectedNums = [...]; // 长度正确的预期答案。
                            // 它以不等于 val 的值排序。

int k = removeElement(nums, val); // 调用你的实现

assert k == expectedNums.length;
sort(nums, 0, k); // 排序 nums 的前 k 个元素
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有的断言都通过，你的解决方案将会 **通过**。

 

**示例 1：**

```
输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2,_,_]
解释：你的函数函数应该返回 k = 2, 并且 nums 中的前两个元素均为 2。
你在返回的 k 个元素之外留下了什么并不重要（因此它们并不计入评测）。
```

**示例 2：**

```
输入：nums = [0,1,2,2,3,0,4,2], val = 2
输出：5, nums = [0,1,4,0,3,_,_,_]
解释：你的函数应该返回 k = 5，并且 nums 中的前五个元素为 0,0,1,3,4。
注意这五个元素可以任意顺序返回。
你在返回的 k 个元素之外留下了什么并不重要（因此它们并不计入评测）。
```

 

**提示：**

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`

题解：



```java
public class Solution3 {
    public int removeElement (int[] nums,int val){
        int ans = 0;
        for(int num:nums){//遍历数组 nums 中的每一个元素
            if(num != val){
                nums[ans] = num;// 将当前元素 num 放入 nums 数组中索引为 ans 的位置
                ans++;
            }
        }
        return ans;
    }
}

```

### 6.leetcode [搜索插入位置](https://leetcode.cn/problems/search-insert-position/)（二分查找）

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 `O(log n)` 的算法。

 

**示例 1:**

```
输入: nums = [1,3,5,6], target = 5
输出: 2
```

**示例 2:**

```
输入: nums = [1,3,5,6], target = 2
输出: 1
```

**示例 3:**

```
输入: nums = [1,3,5,6], target = 7
输出: 4
```

 

**提示:**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` 为 **无重复元素** 的 **升序** 排列数组
- `-104 <= target <= 104`

题解：

使用二分查找

```java
class Solution {
    public int searchInsert(int[] nums, int targer) {
        if (nums == null || nums.length == 0) {
            return 0;
        }//排除0和null的情况
        int left = 0;
        int right = nums.length - 1;
        while (left < right) {
            int mid = (left + right) / 2;//建立二分查找
            if (targer == nums[mid]) {
                return mid;
            } else if (targer < nums[mid]) {
                right = mid - 1;//向左移动
            } else {
                left = mid + 1;//向右移动
            }

        }
        return targer <= nums[left] ? left : left + 1;//最后输出数值，按照左边的顺序
    }
}
```

7. #### leetcode [最后一个单词的长度](https://leetcode.cn/problems/length-of-last-word/)

给你一个字符串 `s`，由若干单词组成，单词前后用一些空格字符隔开。返回字符串中 **最后一个** 单词的长度。

**单词** 是指仅由字母组成、不包含任何空格字符的最大

子字符串

**示例 1：**

```
输入：s = "Hello World"
输出：5
解释：最后一个单词是“World”，长度为 5。
```

**示例 2：**

```
输入：s = "   fly me   to   the moon  "
输出：4
解释：最后一个单词是“moon”，长度为 4。
```

**示例 3：**

```
输入：s = "luffy is still joyboy"
输出：6
解释：最后一个单词是长度为 6 的“joyboy”。
```

 

**提示：**

- `1 <= s.length <= 104`
- `s` 仅有英文字母和空格 `' '` 组成
- `s` 中至少存在一个单词

题解：

```java
class Solution5 {
    public int lengthOfLastWord(String s) {
        int end = s.length() - 1;
        while (end >= 0 && s.charAt(end) == ' ') end--;//从末尾开始，跳过空格
        if (end < 0) return 0;
        int start = end;
        while (start >= 0 && s.charAt(start) != ' ') start--;//从最后一个单词的结尾开始，找到开始位置
        return end - start;

    }
}


```

### 7.leetcode 加一（遍历）

给定一个由 **整数** 组成的 **非空** 数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储**单个**数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

 

**示例 1：**

```
输入：digits = [1,2,3]
输出：[1,2,4]
解释：输入数组表示数字 123。
```

**示例 2：**

```
输入：digits = [4,3,2,1]
输出：[4,3,2,2]
解释：输入数组表示数字 4321。
```

**示例 3：**

```
输入：digits = [0]
输出：[1]
```

 

**提示：**

- `1 <= digits.length <= 100`
- `0 <= digits[i] <= 9`

题解：

```
public class Solution {
    public int[] plusOne(int[] digits){
        for(int i = digits.length-1;i>=0;i--){//从最后一个开始遍历
            digits[i]++;
            digits[i] = digits[i] % 10;
            if(digits[i] != 0)return digits;
        }
        digits = new int[digits.length + 1];//如果遍历完了，还需要进位的话，长度加1，第一位变成1
        digits[0] = 1;
        return digits;
    }
}

```

### 8.leetcode [二进制求和](https://leetcode.cn/problems/add-binary/)(二进制的转化)

给你两个二进制字符串 `a` 和 `b` ，以二进制字符串的形式返回它们的和。

 

**示例 1：**

```
输入:a = "11", b = "1"
输出："100"
```

**示例 2：**

```
输入：a = "1010", b = "1011"
输出："10101"
```

 

**提示：**

- `1 <= a.length, b.length <= 104`
- `a` 和 `b` 仅由字符 `'0'` 或 `'1'` 组成
- 字符串如果不是 `"0"` ，就不含前导零

题解：

```java
class Solution6{
    public String addBinary(String a,String b){
        StringBuilder ans = new StringBuilder();
        int ca = 0;//初始化
        for(int i = a.length() - 1,j=b.length() - 1;i>=0||j>=0;i--,j--){//从最后一位开始往前递归
            int sum = ca;
            sum += i>=0 ? a.charAt(i) - '0' : 0;//往前一直a.charAt(i) - '0' 将字符 '0' 或 '1' 转换为整数 0 或 1。
            sum += j>=0 ? b.charAt(j) - '0' : 0;
            ans.append(sum % 2);
            ca = sum /2;//更新下一位
        }
        ans.append(ca == 1?ca : "");
        return ans.reverse().toString();
    }
}
```

### 9.leetcode [x 的平方根 ](https://leetcode.cn/problems/sqrtx/)（牛顿迭代法 暴力）

给你一个非负整数 `x` ，计算并返回 `x` 的 **算术平方根** 。

由于返回类型是整数，结果只保留 **整数部分** ，小数部分将被 **舍去 。**

**注意：**不允许使用任何内置指数函数和算符，例如 `pow(x, 0.5)` 或者 `x ** 0.5` 。

 

**示例 1：**

```
输入：x = 4
输出：2
```

**示例 2：**

```
输入：x = 8
输出：2
解释：8 的算术平方根是 2.82842..., 由于返回类型是整数，小数部分将被舍去。
```

 

**提示：**

- `0 <= x <= 231 - 1`

题解：

牛顿迭代法

首先随便猜一个近似值 *x*，然后不断令 *x* 等于 *x* 和 *a*/*x* 的平均数，迭代个六七次后 *x* 的值就已经相当精确了。

```java
public class Solution7 {
    int s;
    public int mySqrt(int x){
        s=x;
        if(x==0) return 0;
        return ((int)(sqrts(x)));//主体过测试的主方法，返回平方
    }
    public double sqrts (double x){//设定方法
        double res = (x+s/x)/2;
        if(res == x){
            return x;
        }
        else{
            return sqrts(res);//一直重复这个过程直到x==res的时候
        }
    }
}

```

还有一种类似的方法

```java
class Solution {
    public int mySqrt(int x) {
        if (x == 0) return 0;
        return (int) sqrt(x, x);
    }

    private double sqrt(double x, double s) {
        double res = (x + s / x) / 2;
        if (Math.abs(res - x) < 1e-7) { // 终止条件，精度控制
            return res;
        } else {
            return sqrt(res, s);
        }
    }
}

```

这个主要是控制了下精度。让他在某个范围内就结束

10.leetcode[爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

 

**示例 1：**

```
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶
```

**示例 2：**

```
输入：n = 3
输出：3
解释：有三种方法可以爬到楼顶。
1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
```

 

**提示：**

- `1 <= n <= 45`

题解：

即 f(n) 为以上两种情况之和，即 f(n)=f(n−1)+f(n−2) ，以上递推性质为斐波那契数列。因此，本题可转化为 求斐波那契数列的第 n 项，区别仅在于初始值不同。

```
public class Solution8 {
    public int climbStairs(int n){
        int a = 1,b = 1,sum;
        for(int i = 0;i<n-1;i++){
            sum = a + b;
            a = b;
            b = sum;//a就是n-2，b就是n-1
        }
        return b;
    }
}

```

### 10.leetcode [删除排序链表中的重复元素](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/)（链表）

给定一个已排序的链表的头 `head` ， *删除所有重复的元素，使每个元素只出现一次* 。返回 *已排序的链表* 。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/04/list1.jpg)

```
输入：head = [1,1,2]
输出：[1,2]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/01/04/list2.jpg)

```
输入：head = [1,1,2,3,3]
输出：[1,2,3]
```

 

**提示：**

- 链表中节点数目在范围 `[0, 300]` 内
- `-100 <= Node.val <= 100`
- 题目数据保证链表已经按升序 **排列**

题解：

```
public class Solution9 {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode cur = head;
        while (cur != null && cur.next != null){
            if(cur.val == cur.next.val){
                cur.next = cur.next.next;
            }
            else {
                cur = cur.next;
            }
        }
        return head;
    }
}
class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}//创建链表

```

一个小测试代码：

```java
// 定义链表节点类
class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}

// 解决方案类
public class Solution9 {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode cur = head;
        while (cur != null && cur.next != null) {
            if (cur.val == cur.next.val) {
                cur.next = cur.next.next;
            } else {
                cur = cur.next;
            }
        }
        return head;
    }

    // 主方法，用于测试
    public static void main(String[] args) {
        // 创建测试链表：1 -> 1 -> 2 -> 3 -> 3
        ListNode head = new ListNode(1);
        head.next = new ListNode(1);
        head.next.next = new ListNode(2);
        head.next.next.next = new ListNode(3);
        head.next.next.next.next = new ListNode(3);

        Solution9 solution = new Solution9();
        ListNode result = solution.deleteDuplicates(head);

        // 输出处理后的链表
        while (result != null) {
            System.out.print(result.val + " ");
            result = result.next;
        }
        // 期望输出：1 2 3
    }
}

```

输出结果和期望一致

🦅了

### 11.leetcode [合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/)

给你两个按 **非递减顺序** 排列的整数数组 `nums1` 和 `nums2`，另有两个整数 `m` 和 `n` ，分别表示 `nums1` 和 `nums2` 中的元素数目。

请你 **合并** `nums2` 到 `nums1` 中，使合并后的数组同样按 **非递减顺序** 排列。

**注意：**最终，合并后数组不应由函数返回，而是存储在数组 `nums1` 中。为了应对这种情况，`nums1` 的初始长度为 `m + n`，其中前 `m` 个元素表示应合并的元素，后 `n` 个元素为 `0` ，应忽略。`nums2` 的长度为 `n` 。

 

**示例 1：**

```
输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
解释：需要合并 [1,2,3] 和 [2,5,6] 。
合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。
```

**示例 2：**

```
输入：nums1 = [1], m = 1, nums2 = [], n = 0
输出：[1]
解释：需要合并 [1] 和 [] 。
合并结果是 [1] 。
```

**示例 3：**

```
输入：nums1 = [0], m = 0, nums2 = [1], n = 1
输出：[1]
解释：需要合并的数组是 [] 和 [1] 。
合并结果是 [1] 。
注意，因为 m = 0 ，所以 nums1 中没有元素。nums1 中仅存的 0 仅仅是为了确保合并结果可以顺利存放到 nums1 中。
```

 

**提示：**

- `nums1.length == m + n`
- `nums2.length == n`
- `0 <= m, n <= 200`
- `1 <= m + n <= 200`
- `-109 <= nums1[i], nums2[j] <= 109`

题解：

```
public class Solution10 {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int i = m - 1; // nums1 中最后一个有效元素的索引
        int j = n - 1; // nums2 中最后一个有效元素的索引
        int index = m + n - 1; // 合并后数组的最后一个位置的索引

        // 从后向前合并两个数组
        while (i >= 0 && j >= 0) {
            if (nums1[i] <= nums2[j]) {
                nums1[index--] = nums2[j--];
            } else {
                nums1[index--] = nums1[i--];
            }
        }

        // 如果 nums2 中还有剩余元素，继续填入 nums1 中
        while (j >= 0) {
            nums1[index--] = nums2[j--];
        }

        // 如果 nums1 中有剩余元素，不需要额外处理，因为它们已经在正确的位置上
    }
}

```

### 12.leetcode [验证回文串](https://leetcode.cn/problems/valid-palindrome/)(双指针)

如果在将所有大写字符转换为小写字符、并移除所有非字母数字字符之后，短语正着读和反着读都一样。则可以认为该短语是一个 **回文串** 。

字母和数字都属于字母数字字符。

给你一个字符串 `s`，如果它是 **回文串** ，返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

```
输入: s = "A man, a plan, a canal: Panama"
输出：true
解释："amanaplanacanalpanama" 是回文串。
```

**示例 2：**

```
输入：s = "race a car"
输出：false
解释："raceacar" 不是回文串。
```

**示例 3：**

```
输入：s = " "
输出：true
解释：在移除非字母数字字符之后，s 是一个空字符串 "" 。
由于空字符串正着反着读都一样，所以是回文串。
```

 

**提示：**

- `1 <= s.length <= 2 * 105`
- `s` 仅由可打印的 ASCII 字符组成

题解：

判断回文一般都是用双指针算法。这题和其他题不同的是有多余空或者符号，我们只要去掉这些多余空格和符号就行，再进行判断。

```
public class Solution11 {
    public boolean isPalindrome(String s) {
        int l = 0, r = s.length() - 1;//前后项

        while (l < r) {
            // 跳过非字母和非数字字符
            while (l < r && !Character.isLetterOrDigit(s.charAt(l))) {
                l++;
            }
            while (l < r && !Character.isLetterOrDigit(s.charAt(r))) {
                r--;
            }

            // 比较字符
            if (l < r) {
                if (Character.toLowerCase(s.charAt(l)) != Character.toLowerCase(s.charAt(r))) {
                    return false;
                }
                l++;
                r--;
            }
        }
        return true;
    }
}

```

### 13.leetocode [汉明距离](https://leetcode.cn/problems/hamming-distance/)

两个整数之间的 [汉明距离](https://baike.baidu.com/item/汉明距离) 指的是这两个数字对应二进制位不同的位置的数目。

给你两个整数 `x` 和 `y`，计算并返回它们之间的汉明距离。

 

**示例 1：**

```
输入：x = 1, y = 4
输出：2
解释：
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑
上面的箭头指出了对应二进制位不同的位置。
```

**示例 2：**

```
输入：x = 3, y = 1
输出：1
```

 

**提示：**

- `0 <= x, y <= 231 - 1`

题解：

一（比特值算法）

```
public class Solution {
    public int hammingDistance(int x, int y){
       int ans = 0;
       for(int i = 0;i<32;i++){
           int a = (x>>i)&1,b=(y>>i) & 1;
           ans +=a^b;
       }
       return ans;
    }
}
```

本身不改变 *x* 和 *y*，每次取不同的偏移位进行比较，不同则加一

详细解析：

**`for (int i = 0; i < 32; i++)`**:

- 这个循环迭代 32 次，每次 `i` 从 0 到 31。对于 32 位整数，这样可以检查所有的比特位。

**`int a = (x >> i) & 1;`**:

- `x >> i` 是将 `x` 右移 `i` 位。比如，如果 `i = 0`，那么 `x >> 0` 就是 `x` 本身；如果 `i = 1`，那么 `x >> 1` 是 `x` 右移 1 位。
- `(x >> i) & 1` 通过位与运算 `& 1` 提取 `x` 在第 `i` 位的值。结果是 `0` 或 `1`，代表第 `i` 位的比特值。

**`int b = (y >> i) & 1;`**:

- 类似地，这一行提取 `y` 在第 `i` 位的比特值。

**`ans += a ^ b;`**:

- `a ^ b` 是异或运算。异或运算的结果是：如果 `a` 和 `b` 不相等，则结果为 `1`；如果 `a` 和 `b` 相等，则结果为 `0`。
- 这行代码将 `a` 和 `b` 的异或结果加到 `ans` 上。每次异或结果为 `1` 表示在这个位上 `x` 和 `y` 的比特不同，增加 `1` 到 `ans` 上。

**`return ans;`**:

- 返回 `ans`，它是 `x` 和 `y` 之间所有不同比特位的总数，即汉明距离。

二（右移统计）

```
public class Solution {
    public int hammingDistance(int x, int y){
       int ans = 0;
       while((x|y)!=0){
           int a = x & 1 ,b = y & 1;
           ans +=a ^b;
           x>>=1;
           y>>=1;
       }
       return ans;

    }
}

```

首先先将xy和1进行位与运算，提取其值为1或者0；

然后对a和b进行异或计算，看是不是相等，，然后将不相等的的和进行计算

然后xy分别向右开始移1位

最后return ans的值；

### 14.leetocode [汉明距离总和](https://leetcode.cn/problems/total-hamming-distance/)

给你一个整数数组 `nums`，请你计算并返回 `nums` 中任意两个数之间 **汉明距离的总和** 。

题解：

一开始我是刚做了上面哪个题，然后我就想着直接沿着上面的思路继续做吧

然后我写了

```
public class Solution12 {
    public int hammingDistance(int x, int y){
       int ans = 0;
       while((x|y)!=0){
           int a = x & 1 ,b = y & 1;
           ans +=a ^b;
           x>>=1;
           y>>=1;
       }
       return ans;
    }
    public  int totalHammingDistance(int[] nums){
        int a = 0;
        int b = 0;
        int ans= 0;
        if(nums!=null){
            for(int num:nums){
                a = num;
            }
            for(int num:nums){
                b = num;
            }
        }
        for(int num:nums) {
            ans += hammingDistance(a, b);
        }
        return ans;
    }
}
```

然后就哈哈了，我这个代码写的根本不对好吧。

然后我就看了wp然后发现人家写的怎么这么简单

```
class Solution {
    public int totalHammingDistance(int[] nums) {
        int ans = 0, n = nums.length;
        for (int i = 0; i < 30; ++i) {
            int c = 0;
            for (int val : nums) {
                c += (val >> i) & 1;
            }
            ans += c * (n - c);
        }
        return ans;
    }
}

```

汉明距离的贡献分析

1. **汉明距离的定义**： 汉明距离是两个数在相同位置的比特位不同的数量。换句话说，计算两个数字在对应位置上不同的比特位数。
2. **逐位分析**： 对于数组中的每一位（假设从第 `i` 位开始），我们需要计算所有数字在该位上为 `1` 和为 `0` 的数量。
   - **`c`**：在第 `i` 位上为 `1` 的数字的数量。
   - **`n - c`**：在第 `i` 位上为 `0` 的数字的数量，其中 `n` 是数组的总长度。
3. **计算该位的汉明距离贡献**： 对于每个数字，在第 `i` 位上为 `1` 的数字将与第 `i` 位上为 `0` 的数字产生不同的比特位。也就是说，对于每个在第 `i` 位上为 `1` 的数字，它都与所有在第 `i` 位上为 `0` 的数字之间产生了一个不同的比特位。因此，每个 `1` 会与每个 `0` 形成一个汉明距离的贡献。
   - 如果 `c` 个数字在第 `i` 位上是 `1`，那么这些 `1` 对每个在第 `i` 位上是 `0` 的数字（共有 `n - c` 个）都有一个不同的比特位。
   - 因此，在第 `i` 位上，汉明距离的总贡献是 `c * (n - c)`。

举个例子

假设数组 `nums` 为 `[1, 4, 2]`，我们要计算汉明距离贡献时考虑每一位的情况：

- **第 0 位**（最低位）：
  - `nums` 中的二进制表示是：`[001, 100, 010]`
  - 在第 0 位上，有 `2` 个 `1`（`001` 和 `101`），`1` 个 `0`（`100`）。
  - 贡献：`2 * (3 - 2) = 2 * 1 = 2`
- **第 1 位**：
  - 二进制表示：`[001, 100, 010]`
  - 在第 1 位上，有 `1` 个 `1`（`010`），`2` 个 `0`（`001` 和 `100`）。
  - 贡献：`1 * (3 - 1) = 1 * 2 = 2`
- **第 2 位**：
  - 二进制表示：`[001, 100, 010]`
  - 在第 2 位上，有 `1` 个 `1`（`100`），`2` 个 `0`（`001` 和 `010`）。
  - 贡献：`1 * (3 - 1) = 1 * 2 = 2`

将所有位上的贡献加起来，总汉明距离是 `2 + 2 + 2 = 6`。

### 15.leetcode[心算挑战](https://leetcode.cn/problems/uOAnQW/)

**暂时未解决**

16.leetcode [采购方案](https://leetcode.cn/problems/4xy4Wx/)

小力将 N 个零件的报价存于数组 `nums`。小力预算为 `target`，假定小力仅购买两个零件，要求购买零件的花费不超过预算，请问他有多少种采购方案。

注意：答案需要以 `1e9 + 7 (1000000007)` 为底取模，如：计算初始结果为：`1000000008`，请返回 `1`

**示例 1：**

> 输入：`nums = [2,5,3,5], target = 6`
>
> 输出：`1`
>
> 解释：预算内仅能购买 nums[0] 与 nums[2]。

**示例 2：**

> 输入：`nums = [2,2,1,9], target = 10`
>
> 输出：`4`
>
> 解释：符合预算的采购方案如下： nums[0] + nums[1] = 4 nums[0] + nums[2] = 3 nums[1] + nums[2] = 3 nums[2] + nums[3] = 10

**提示：**

- `2 <= nums.length <= 10^5`
- `1 <= nums[i], target <= 10^5`

题解：

使用双指针

```
import java.util.Arrays;

class Solution {
    public int purchasePlans(int[] nums, int target) {
        int mod = 1_000_000_007;
        int ans = 0;
        Arrays.sort(nums);
        int left = 0;int right = nums.length - 1;
        while (left<right){
            if (nums[left]+nums[right]>target) right--;//说明不符合次数的条件
            else {
                ans += right - left;//否则则说明符合，将中间的情况都加起来
                left++;//向左移动指针继续计算
            }
            ans %= mod;
        }
        return ans % mod;
    }
}
```

