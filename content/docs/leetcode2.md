---
title: Leetcode滑动窗口双指针转型专项
categories:
  - java
  
abbrlink: 2701
date: 2025.4.29
tags: 
   - java 
   - 数据结构
   - leetcode
   - 算法
---

# 定长型

定长型：

直接考虑头和尾就行。

只需要考虑**移除（离开窗口）的字母 a 是不是元音，以及添加（进入窗口）的字母 i 是不是元音即可**

然后考虑窗口大小不足的时候,continue

## [1456. 定长子串中元音的最大数目](https://leetcode.cn/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)

给你字符串 `s` 和整数 `k` 。

请返回字符串 `s` 中长度为 `k` 的单个子字符串中可能包含的最大元音字母数。

英文中的 **元音字母** 为（`a`, `e`, `i`, `o`, `u`）。



解决定长窗口的问题：

只需要考虑**移除（离开窗口）的字母 a 是不是元音，以及添加（进入窗口）的字母 i 是不是元音即可**



```java
class Solution1456{
    public int maxVowels(String S,int k ){
        char[] s = S.toCharArray();
        int ans  = 0;
        int vowel  = 0;

        for (int i =0;i<s.length;i++){
            if (s[i] == 'a' || s[i] == 'e' || s[i] == 'i' || s[i] == 'o' || s[i] == 'u'){
                vowel++;
            }
            if (i<k-1){//窗口大小不到k
                continue;
            }
            ans = Math.max(ans,vowel);
            char out = s[i-k+1];
            if (out == 'a' || out == 'e' || out == 'i' || out == 'o' || out == 'u'){
                vowel--;
            }
        }
        return ans;

    }
}
```

就是这个用法。

## [643. 子数组最大平均数 I](https://leetcode.cn/problems/maximum-average-subarray-i/)

给你一个由 `n` 个元素组成的整数数组 `nums` 和一个整数 `k` 。

请你找出平均数最大且 **长度为 `k`** 的连续子数组，并输出该最大平均数。

任何误差小于 `10-5` 的答案都将被视为正确答案。



这也是一个定长的滑动窗口，按照模板做就行，维护一个最大值即可

```java
class Solution643{
    public double findMaxAverage(int[] nums, int k){
        int maxS = Integer.MIN_VALUE;
        int s=  0;
        for (int i =0;i<nums.length;i++){
            s +=nums[i];
            if (i<k-1){
                continue;
            }
            maxS = Math.max(maxS,s);
            s -=nums[i-k+1];
        }
        return (double) maxS/k;
    }
}

```

示例代码如下

## [1343. 大小为 K 且平均值大于等于阈值的子数组数目](https://leetcode.cn/problems/number-of-sub-arrays-of-size-k-and-average-greater-than-or-equal-to-threshold/)

给你一个整数数组 `arr` 和两个整数 `k` 和 `threshold` 。

请你返回长度为 `k` 且平均值大于等于 `threshold` 的子数组数目。

这个题在上面的基础上改就行

```java
class Solution1343{
    public int numOfSubarrays(int[] arr, int k, int threshold){
        int ans = 0;
        int s=  0;
        for (int i =0;i<arr.length;i++){
            s +=arr[i];
            if (i<k-1){
                continue;
            }
            if (s>=threshold*k){
                ans++;
            }
            s -=arr[i-k+1];
        }
        return ans;
    }
}

```

这里不是求最大了，是每一个都求，所以不用maxS了

## [2090. 半径为 k 的子数组平均值](https://leetcode.cn/problems/k-radius-subarray-averages/)

给你一个下标从 **0** 开始的数组 `nums` ，数组中有 `n` 个整数，另给你一个整数 `k` 。

**半径为 k 的子数组平均值** 是指：`nums` 中一个以下标 `i` 为 **中心** 且 **半径** 为 `k` 的子数组中所有元素的平均值，即下标在 `i - k` 和 `i + k` 范围（**含** `i - k` 和 `i + k`）内所有元素的平均值。如果在下标 `i` 前或后不足 `k` 个元素，那么 **半径为 k 的子数组平均值** 是 `-1` 。

构建并返回一个长度为 `n` 的数组 `avgs` ，其中 `avgs[i]` 是以下标 `i` 为中心的子数组的 **半径为 k 的子数组平均值** 。

`x` 个元素的 **平均值** 是 `x` 个元素相加之和除以 `x` ，此时使用截断式 **整数除法** ，即需要去掉结果的小数部分。

- 例如，四个元素 `2`、`3`、`1` 和 `5` 的平均值是 `(2 + 3 + 1 + 5) / 4 = 11 / 4 = 2.75`，截断后得到 `2` 

本题相当于一个长为 2*k*+1 的滑动窗口

```java
class Solution2090{
    public int[] getAveragesA(int[] nums, int k){
        int n = nums.length;
        int[] avgs = new int [n];
        Arrays.fill(avgs,-1);
        long s= 0;
        for (int i=0;i<n;i++){
            s +=nums[i];
            if (i<k*2){
                continue;
            }
            avgs[i-k] = (int)(s/(k*2+1));
            s -=nums[i-k*2];
        }
        return avgs;
    }
    public int[] getAverages(int[] nums, int k){
        int n = nums.length;
        int l = 2*k+1;
        int[] res = new int [n];
        for (int i =0;i<n;i++){
            res[i] = -1;
        }
        if (l>n){
            return res;
        }
        long sum  = 0;
        for (int i=0;i<l;i++){
            sum +=nums[i];
        }
        res[k] = (int)(sum/l);
        for (int i=l;i<n;i++){
            sum +=nums[i]-nums[i-l];
            res[i-k] = (int)(sum/l);
        }
        return res;
    }
}
```

还是那种做法，这里提供了两种写法

## [2841. 几乎唯一子数组的最大和](https://leetcode.cn/problems/maximum-sum-of-almost-unique-subarray/)

给你一个整数数组 `nums` 和两个正整数 `m` 和 `k` 。

请你返回 `nums` 中长度为 `k` 的 **几乎唯一** 子数组的 **最大和** ，如果不存在几乎唯一子数组，请你返回 `0` 。

如果 `nums` 的一个子数组有至少 `m` 个互不相同的元素，我们称它是 **几乎唯一** 子数组。

子数组指的是一个数组中一段连续 **非空** 的元素序列。



找个时候就要用到hashmap因为hashmap是不重复的，可以统计几乎唯一数组中元素的数量

```java
class Solution2841{
    public long maxSum(List<Integer> nums, int m, int k){
        Integer[] a = nums.toArray(new Integer[0]);
        long ans= 0;
        long s =0;
        Map<Integer,Integer> cnt  = new HashMap<>();

        for (int i =0;i<a.length;i++){
            s +=a[i];
            cnt.merge(a[i],1,Integer::sum);
            int left = i-k+1;
            if (left<0){
                continue;
            }
            if (cnt.size()>=m){
                ans = Math.max(ans,s);
            }
            int out = a[left];
            s -=out;
            int c = cnt.get(out);
            if (c>1){
                cnt.put(out,c-1);//出现次数-1
            }else {
                cnt.remove(out);//出现次数为0了移除
            }

        }
        return ans;
    }
}

```

这里维护cnt为存在不同的元素

ans为结果

s为和

## [3439. 重新安排会议得到最多空余时间 I](https://leetcode.cn/problems/reschedule-meetings-for-maximum-free-time-i/)

给你一个整数 `eventTime` 表示一个活动的总时长，这个活动开始于 `t = 0` ，结束于 `t = eventTime` 。

同时给你两个长度为 `n` 的整数数组 `startTime` 和 `endTime` 。它们表示这次活动中 `n` 个时间 **没有重叠** 的会议，其中第 `i` 个会议的时间为 `[startTime[i], endTime[i]]` 。

你可以重新安排 **至多** `k` 个会议，安排的规则是将会议时间平移，且保持原来的 **会议时长** ，你的目的是移动会议后 **最大化** 相邻两个会议之间的 **最长** 连续空余时间。

移动前后所有会议之间的 **相对** 顺序需要保持不变，而且会议时间也需要保持互不重叠。

请你返回重新安排会议以后，可以得到的 **最大** 空余时间。

**注意**，会议 **不能** 安排到整个活动的时间以外。

```java
class Solution3439 {
    public int maxFreeTime(int eventTime, int k, int[] startTime, int[] endTime) {
        int ans = 0;
        int s = 0;
        for (int i = 0; i <= startTime.length; i++) {
            s += get(i, eventTime, startTime, endTime);
            if (i >= k) {
                ans = Math.max(ans, s); // ✅ 更新最大值
                s -= get(i - k, eventTime, startTime, endTime);
            }
        }
        return ans;
    }

    private int get(int i, int eventTime, int[] startTime, int[] endTime) {
        if (i == 0) {
            return startTime[0]; // 第一个活动前的空闲时间
        }
        int n = startTime.length;
        if (i == n) {
            return eventTime - endTime[n - 1]; // 所有活动结束后的空闲时间
        }
        return startTime[i] - endTime[i - 1]; // 活动之间的空闲时间
    }
}
```

还是经典的定长滑动窗口问题，这里进出获取是取得他的空闲时间

如果是没有会议了，就返回第一个活动前的空闲时间

在最后的时候，就返回会议结束后的空闲时间

普通的返回，这个开始喝上一个结束之间的空闲时间

## [438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)

给定两个字符串 `s` 和 `p`，找到 `s` 中所有 `p` 的 **异位词** 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

```java
class Solution438{
    public List<Integer> findAnagramsA(String s, String p){
        List<Integer> ans = new ArrayList<>();
        if (s.length()<p.length()) return ans;
        int[] cnt = new int[26];
        for (char c:p.toCharArray()){
            cnt[c-'a']++;
        }
        int left = 0,right  = 0,required= p.length();
        while (right<s.length()){
            int c = s.charAt(required)-'a';
            if (cnt[c]>0){
                required--;
            }
            cnt[c]--;
            right++;
            if (required==0){
                ans.add(left);
            }
            if (right-left==p.length()){
                int l = s.charAt(left)-'a';
                if (cnt[l]>=0){
                    required++;
                }
                cnt[l]++;
                left++;
            }
        }
        return ans;
    }
    public List<Integer> findAnagrams(String s, String p){
        List<Integer> ans = new ArrayList<>();
        int[] cntP = new int[26];//p种每种字母的出现次数
        int[] cntS = new int[26];//子串每种字母的出现次数
        for (char c:p.toCharArray()){
            cntP[c-'a']++;
        }
        for (int right=0;right<s.length();right++){
            cntS[s.charAt(right)-'a']++;
            int left = right-p.length()+1;
            if (left<0){
                continue;
            }
            if (Arrays.equals(cntP,cntS)){
                ans.add(left);
            }
            cntS[s.charAt(left)-'a']--;
        }
        return ans;
    }
}
```

在这里还是使用定长的滑动窗口的方法

字符进入窗口，看尾端是不是小于0了，小于就跳过继续

看窗口之中 *p* 的每种字母的出现次数和长为 *n* 的子串 *s*′ 的每种字母的出现次数相等的时候

就把他起始的索引left加入到ans中

然后左边left出去窗口

最后返回答案ans

## [567. 字符串的排列](https://leetcode.cn/problems/permutation-in-string/)

给你两个字符串 `s1` 和 `s2` ，写一个函数来判断 `s2` 是否包含 `s1` 的 排列。如果是，返回 `true` ；否则，返回 `false` 。

换句话说，`s1` 的排列之一是 `s2` 的 **子串** 

```java
class Solution567{
    public boolean checkInclusion(String s1, String s2){
        int n  = s1.length();
        int m = s2.length();
        if (n>m){
            return false;
        }
        int[] cnt = new int[26];
        for (char c:s1.toCharArray()){
            cnt[c-'a']++;
        }
        int[] cur = new int[26];
        for (int i =0;i<n;i++){
            cur[s2.charAt(i)-'a']++;
        }
        if (check(cnt,cur)){
            return true;
        }
        for (int i =n;i<m;i++){
            cur[s2.charAt(i)-'a']++;
            cur[s2.charAt(i-n)-'a']--;
            if (check(cnt,cur)){
                return true;
            }
        }
        return false;
    }
    private boolean check(int[] cnt1, int[] cnt2){
        for (int i=0;i<26;i++){
            if (cnt1[i]!=cnt2[i]){
                return false;
            }
        }
        return true;
    }
}
```

还是经典的滑动窗口问题

维护cnt为s1中各个字母的出现频率

cur为s2中长度为n的字母的频度

check函数就是看cnt和cur是不是相等，相等的话，返回true，不相等返回false

然后滑动窗口进入i,cur头和尾部分别开始变化，然后再去比较。

相等返回true,不相等返回false

这种一个定，另一个滑动的写法也是可以的

```java
class Solution567{
    public boolean checkInclusionA(String s1, String s2){
        int n  = s1.length();
        int m = s2.length();
        if (n>m){
            return false;
        }
        int[] cnt = new int[26];
        for (char c:s1.toCharArray()){
            cnt[c-'a']++;
        }
        int[] cur = new int[26];
        for (int i =0;i<n;i++){
            cur[s2.charAt(i)-'a']++;
        }
        if (check(cnt,cur)){
            return true;
        }
        for (int i =n;i<m;i++){
            cur[s2.charAt(i)-'a']++;
            cur[s2.charAt(i-n)-'a']--;
            if (check(cnt,cur)){
                return true;
            }
        }
        return false;
    }
    private boolean check(int[] cnt1, int[] cnt2){
        for (int i=0;i<26;i++){
            if (cnt1[i]!=cnt2[i]){
                return false;
            }
        }
        return true;
    }
    public boolean checkInclusion(String s1, String s2){
        int n =s1.length(),m= s2.length();
        if (n>m) return false;
        int[] cntS1 = new int[26];
        int[] cntS2 = new int[26];

        for (char c:s1.toCharArray()){
            cntS1[c-'a']++;
        }
        for (int right=0;right<m;right++){
            cntS2[s2.charAt(right)-'a']++;
            int left =right-n+1;
            if (left<0){
                continue;
            }
            if (Arrays.equals(cntS1,cntS2)){
                return true;
            }
            cntS2[s2.charAt(left)-'a']--;
        }
        return false;
    }
}

```

另一种写法。

# 不定长窗口

求最长子数组，求最短子数组，以及求子数组个数。

滑动窗口相当于在维护一个**队列**。右指针的移动可以视作**入队**，左指针的移动可以视作**出队**。

## 求最大

### [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长 子串** 的长度。

```java
class Solution3{
    public int lengthOfLongestSubstring(String s){
        int n = s.length(),ans = 0;
        Map<Character,Integer> map = new HashMap<>();
        for (int end= 0,start = 0;end<n;end++){
            char alpha = s.charAt(end);
            if (map.containsKey(alpha)){
                start = Math.max(map.get(alpha),start);
            }
            ans = Math.max(ans,end-start+1);
            map.put(s.charAt(end),end+1);
        }
        return ans;
    }
}
```

一想到字符我们就想到了hashmap

这里key为字符，value为最后一次出现的索引

遇到重复的就，更新字符索引的最大值。

然后最后无重复字符的长度就是end-start+1

然后更新map，将end往后移动

最后返回ans

### [3090. 每个字符最多出现两次的最长子字符串](https://leetcode.cn/problems/maximum-length-substring-with-two-occurrences/)

给你一个字符串 `s` ，请找出满足每个字符最多出现两次的最长子字符串，并返回该子字符串的 **最大** 长度

```java
class Solution3090{
    public int maximumLengthSubstring(String S){
        char[] s=S.toCharArray();
        int ans = 0;
        int left = 0;
        int[] cnt = new int [26];
        for (int i=0;i<s.length;i++){
            int b = s[i]-'a';
            cnt[b]++;
            while (cnt[b]>2){
                cnt[s[left++]-'a']--;
            }
            ans = Math.max(ans,i-left+1);
                }
        return ans;
    }
}
```

还是经典的滑动窗口，当cnt中存储的字符子树大于2次之后。

移除左边的字符，因为超过2了

然后缩小窗口。

最后长度就是i也就是到最后窗口里面的

### [1493. 删掉一个元素以后全为 1 的最长子数组](https://leetcode.cn/problems/longest-subarray-of-1s-after-deleting-one-element/)

给你一个二进制数组 `nums` ，你需要从中删掉一个元素。

请你在删掉元素的结果数组中，返回最长的且只包含 1 的非空子数组的长度。

如果不存在这样的子数组，请返回 0 

```java
class Solution1493{
   public int longestSubarray(int[] nums){
        int n  = nums.length;
        int l=-1,zero = -1;
        int ans = 0;
        for (int i =0;i<n;i++){
            if (nums[i]==0){
                l = zero+1;
                zero = i;
            }
            ans = Math.max(ans,i-l);
        }
        return Math.min(ans,n-1);
    }
}
```

因为求的是最长的那个

肯定是在两个0中间，然后去掉中间的0就是这个数组了

l为第一个不为0的数

zero为上一个0

i-l为这个数组的长度

然后可能这个数组就只有一个0，所以可能也是n-1，和ans比较一下

### [904. 水果成篮](https://leetcode.cn/problems/fruit-into-baskets/)

你正在探访一家农场，农场从左到右种植了一排果树。这些树用一个整数数组 `fruits` 表示，其中 `fruits[i]` 是第 `i` 棵树上的水果 **种类** 。

你想要尽可能多地收集水果。然而，农场的主人设定了一些严格的规矩，你必须按照要求采摘水果：

- 你只有 **两个** 篮子，并且每个篮子只能装 **单一类型** 的水果。每个篮子能够装的水果总量没有限制。
- 你可以选择任意一棵树开始采摘，你必须从 **每棵** 树（包括开始采摘的树）上 **恰好摘一个水果** 。采摘的水果应当符合篮子中的水果类型。每采摘一次，你将会向右移动到下一棵树，并继续采摘。
- 一旦你走到某棵树前，但水果不符合篮子的水果类型，那么就必须停止采摘。

给你一个整数数组 `fruits` ，返回你可以收集的水果的 **最大** 数目。

```java
class Solution940{
    public int totalFruit(int[] fruits){
        int ans=  0;
        int left  =0;
        Map<Integer,Integer> cnt = new HashMap<>();
        for (int right = 0;right<fruits.length;right++){
            cnt.merge(fruits[right],1,Integer::sum);
            while (cnt.size()>2){
                int out = fruits[left];
                cnt.merge(out,-1,Integer::sum);
                if (cnt.get(out)==0){
                    cnt.remove(out);
                }
                left++;
            }
            ans = Math.max(ans,right-left+1);
        }
        return ans;
    }
}
```

一看就是滑动窗口，而且说没用重复，就是用上hashmap

right进入窗口，cnt记录

如果cnt.size大于2了

说明left端点该-1

如果left为0了，那么就从cnt中移除

然后缩小窗口，最后窗口的长度就是数目。

更新即可

### [1695. 删除子数组的最大得分](https://leetcode.cn/problems/maximum-erasure-value/)

给你一个正整数数组 `nums` ，请你从中删除一个含有 **若干不同元素** 的子数组**。**删除子数组的 **得分** 就是子数组各元素之 **和** 。

返回 **只删除一个** 子数组可获得的 **最大得分** *。*

如果数组 `b` 是数组 `a` 的一个连续子序列，即如果它等于 `a[l],a[l+1],...,a[r]` ，那么它就是 `a` 的一个子数组。

```java
class Solution1695{
    public int maximumUniqueSubarrayA(int[] nums){
        int n = nums.length;
        Map<Integer,Integer> map = new HashMap<>();
        int l =0,sum = 0,ans = 0;
        for (int i =0;i<n;i++){
            sum +=nums[i];
            if(map.merge(nums[i],1,Integer::sum)==1){
                ans = Math.max(ans,sum);
            }else {
                while (true){
                    if (map.get(nums[l])==1){
                        sum -=nums[l];
                    }else {
                        sum -=nums[l];
                        map.merge(nums[l++],-1,Integer::sum);
                        break;
                    }
                }
            }
        }
        return ans;
    }
    public int maximumUniqueSubarray(int[] nums){
        int sum = 0;
        int max = 0;
        HashSet<Integer> set = new HashSet<Integer>();
        int left = 0;
        for (int right = 0;right<nums.length;right++){
            while (set.contains(nums[right])){
                sum -=nums[left];
                set.remove(nums[left]);
                left++;
            }
            set.add(nums[right]);
            sum +=nums[right];
            max = Math.max(max,sum);
        }
        return max;
    }
}
```

基本的思路就是，遇到已经存储的数据的话，就将这个数据移除，然后缩小窗口。

然后把right新加入的加入到set里面。计算sum

### [2958. 最多 K 个重复元素的最长子数组](https://leetcode.cn/problems/length-of-longest-subarray-with-at-most-k-frequency/)

给你一个整数数组 `nums` 和一个整数 `k` 。

一个元素 `x` 在数组中的 **频率** 指的是它在数组中的出现次数。

如果一个数组中所有元素的频率都 **小于等于** `k` ，那么我们称这个数组是 **好** 数组。

请你返回 `nums` 中 **最长好** 子数组的长度。

**子数组** 指的是一个数组中一段连续非空的元素序列。





经典的滑动窗口问题，看到频度这个就知道要使用hashmap来统计

然后hashmap记录的频度大于k的时候，出现频度-1，然后窗口缩小

最后长度为滑动窗口的长度

```java
class Solution2958{
    public int maxSubarrayLength(int[] nums, int k){
        int ans = 0, left= 0;
        Map<Integer,Integer> cnt = new HashMap<>();
        for (int right = 0;right<nums.length;right++){
            cnt.merge(nums[right],1,Integer::sum);
            while (cnt.get(nums[right])>k){
                cnt.merge(nums[left++],-1,Integer::sum);
            }
            ans = Math.max(ans,right-left+1);
        }
        return ans;
    }
}
```

### [2024. 考试的最大困扰度](https://leetcode.cn/problems/maximize-the-confusion-of-an-exam/)

一位老师正在出一场由 `n` 道判断题构成的考试，每道题的答案为 true （用 `'T'` 表示）或者 false （用 `'F'` 表示）。老师想增加学生对自己做出答案的不确定性，方法是 **最大化** 有 **连续相同** 结果的题数。（也就是连续出现 true 或者连续出现 false）。

给你一个字符串 `answerKey` ，其中 `answerKey[i]` 是第 `i` 个问题的正确结果。除此以外，还给你一个整数 `k` ，表示你能进行以下操作的最多次数：

- 每次操作中，将问题的正确答案改为 `'T'` 或者 `'F'` （也就是将 `answerKey[i]` 改为 `'T'` 或者 `'F'` ）。

请你返回在不超过 `k` 次操作的情况下，**最大** 连续 `'T'` 或者 `'F'` 的数目

```java
class Solution2024{
    public int maxConsecutiveAnswers(String answerKey, int k){
        char[] s= answerKey.toCharArray();
        int ans = 0;
        int left  =0;
        int[] cnt = new int [2];
        for (int right = 0;right<s.length;right++){
            cnt[s[right] >>1&1]++;
            while (cnt[0]>k&&cnt[1]>k){
                cnt[s[left++]>>1&1]--;
            }
            ans  = Math.max(ans,right-left+1);
        }
        return ans;
    }
}
```

题目还是那样，超过了临界值就缩小窗口。唯一不同的是

用位运算 `s[right] >> 1 & 1`，将字符 `'T'` 和 `'F'` 映射为 1 和 0：

### [1004. 最大连续1的个数 III](https://leetcode.cn/problems/max-consecutive-ones-iii/)

给定一个二进制数组 `nums` 和一个整数 `k`，假设最多可以翻转 `k` 个 `0` ，则返回执行操作后 *数组中连续 `1` 的最大个数* 。

```java
class Solution1004{
    public int longestOnes(int[] nums, int k){
        int ans=0,cnt0= 0,left=0;
        for (int right = 0;right<nums.length;right++){
            cnt0 +=1-nums[right];
            while (cnt0>k){
                cnt0 -=1-nums[left++];
            }
            ans = Math.max(ans,right-left+1);
        }
        return ans;
    }
}

```

还是经典的滑动窗口问题，这个题和上个题一样

把翻转变成求<=k下数组的长度

这段代码：

```java
cnt0 += 1 - nums[right];
```

的意思是：**如果 `nums[right] == 0`，就让 `cnt0 += 1`；如果 `nums[right] == 1`，就让 `cnt0 += 0`**。

相当于：

if (nums[right] == 0) cnt0++;

## 求最小

### [209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

给定一个含有 `n` 个正整数的数组和一个正整数 `target` **。**

找出该数组中满足其总和大于等于 `target` 的长度最小的 **子数组** `[numsl, numsl+1, ..., numsr-1, numsr]` ，并返回其长度**。**如果不存在符合条件的子数组，返回 `0` 。

```java
class Solution209{
    public int minSubArrayLen(int target, int[] nums){
        int n = nums.length;
        int ans = n+1;
        int sum = 0,left = 0;
        for (int right = 0;right<n;right++){
            sum +=nums[right];
            while (sum>target){
                ans = Math.min(ans,right-left+1);
                sum -=nums[left++];
            }
        }
        return ans<=n?ans:0;
    }
}
```

还是经典的滑动窗口问题，在这里只不过变成最小了而已。

### [2904. 最短且字典序最小的美丽子字符串](https://leetcode.cn/problems/shortest-and-lexicographically-smallest-beautiful-string/)

给你一个二进制字符串 `s` 和一个正整数 `k` 。

如果 `s` 的某个子字符串中 `1` 的个数恰好等于 `k` ，则称这个子字符串是一个 **美丽子字符串** 。

令 `len` 等于 **最短** 美丽子字符串的长度。

返回长度等于 `len` 且字典序 **最小** 的美丽子字符串。如果 `s` 中不含美丽子字符串，则返回一个 **空** 字符串。

对于相同长度的两个字符串 `a` 和 `b` ，如果在 `a` 和 `b` 出现不同的第一个位置上，`a` 中该位置上的字符严格大于 `b` 中的对应字符，则认为字符串 `a` 字典序 **大于** 字符串 `b` 。

- 例如，`"abcd"` 的字典序大于 `"abcc"` ，因为两个字符串出现不同的第一个位置对应第四个字符，而 `d` 大于 `c` 。

```java
class Solution2904{
    public String shortestBeautifulSubstring(String S, int k){
        if (S.replace("0","").length()<k){
            return "";
        }
        char[] s = S.toCharArray();
        String ans = S;
        int cnt1 = 0,left = 0;
        for (int right = 0;right<s.length;right++){
            cnt1 +=s[right]-'0';
            while (cnt1>k||s[left]=='0'){
                cnt1 -=s[left++]-'0';
            }
            if (cnt1==k){
                String t = S.substring(left,right+1);
                if (t.length()<ans.length()||t.length()==ans.length()&&t.compareTo(ans)<0){
                    ans = t;
                }
            }
        }
        return ans;
    }
}
```

还是经典的滑动窗口这里找的是正好1的数目正好是k的

然后1的出现次数超过k或者窗口值left出现0就要缩小窗口

然后找到正好是K的值返回

### [76. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)（x）

给你一个字符串 `s` 、一个字符串 `t` 。返回 `s` 中涵盖 `t` 所有字符的最小子串。如果 `s` 中不存在涵盖 `t` 所有字符的子串，则返回空字符串 `""` 。

*s* 的子串 BANC 中每个字母的出现次数，都**大于等于** *t*=ABC 中每个字母的出现次数，这就叫**涵盖**。



## 求数目

#### 越长越合法

内层循环结束后，[left,right] 这个子数组是不满足题目要求的，但在退出循环之前的最后一轮循环，[left−1,right] 是满足题目要求的。由于子数组越长，越能满足题目要求，所以除了 [left−1,right]，还有 [left−2,right],[left−3,right],…,[0,right] 都是满足要求的。也就是说，当右端点固定在 right 时，左端点在 0,1,2,…,left−1 的所有子数组都是满足要求的，这一共有 **left** 个。

##### [1358. 包含所有三种字符的子字符串数目](https://leetcode.cn/problems/number-of-substrings-containing-all-three-characters/)

给你一个字符串 `s` ，它只包含三种字符 a, b 和 c 。

请你返回 a，b 和 c 都 **至少** 出现过一次的子字符串数目。

```java
class Solution1358{
    public int numberOfSubstrings(String S){
        char[] s = S.toCharArray();
        int ans = 0;
        int left = 0;
        int[] cnt = new int[3];
        for (char c:s){
            cnt[c-'a']++;
            while (cnt[0]>0&&cnt[1]>0&&cnt[2]>0){
                cnt[s[left]-'a']--;//左边端口值的出现次数减少
                left++;//收缩窗口
            }
            ans +=left;
        }
        return ans;
    }
}
```

这个类似模板。

但在退出循环之前的最后一轮循环，[*left*−1,*right*] 是满足题目要求的。

所以这个left才是答案。

##### [2962. 统计最大元素出现至少 K 次的子数组](https://leetcode.cn/problems/count-subarrays-where-max-element-appears-at-least-k-times/)

给你一个整数数组 `nums` 和一个 **正整数** `k` 。

请你统计有多少满足 「 `nums` 中的 **最大** 元素」至少出现 `k` 次的子数组，并返回满足这一条件的子数组的数目。

子数组是数组中的一个连续元素序列。



这个题目主要是两步，先找出最大元素，然后至少出现k次的数组，使用越长越合法的滑动窗口解决

```java
class Solution2962A{
    public long countSubarrays(int[] nums, int k){
        int mx = 0;
        for (int x:nums){
            mx = Math.max(mx,x);
        }
        long ans  =0;
        int cntMx=  0,left = 0;
        for (int x:nums){
            if (x==mx){
                cntMx++;
            }
            while (cntMx==k){
                if (nums[left]==mx){
                    cntMx--;
                }
                left++;
            }
            ans +=left;
        }
        return ans;
    }
}
```

这里我们再偷偷使用双指针区解决一下试试：


```java
class Solution2926B{
    public long countSubarrays(int[] nums, int k){
        int mx = Arrays.stream(nums).max().getAsInt();
        int n = nums.length;
        long ans = 0;
        int cnt =0,left = 0;
        for (int x:nums){
            while (left<n&&cnt>k){
                cnt +=nums[left++]==mx?1:0;
            }
            if (cnt<k){
                break;
            }
            ans +=n-left+1;
            cnt -=x==mx?1:0;
        }
        return ans;
    }
    
}
```

##### [3325. 字符至少出现 K 次的子字符串 I](https://leetcode.cn/problems/count-substrings-with-k-frequency-characters-i/)

给你一个字符串 `s` 和一个整数 `k`，在 `s` 的所有子字符串中，请你统计并返回 **至少有一个** 字符 **至少出现** `k` 次的子字符串总数。

**子字符串** 是字符串中的一个连续、 **非空** 的字符序列。



这里就新建一个int[]数组，存放字母的出现次数，然后至少left那个可以就行了

然后缩窗口

```java
class Solution3325{
    int numberOfSubstrings(String S, int k){
        char[] s= S.toCharArray();
        int ans= 0 ;
        int left = 0;
        int [] cnt = new int[26];
        for (char c:s){
            cnt[c-'a']++;
            while (cnt[c-'a']>=k){
                cnt[s[left]-'a']--;
                left++;
            }
            ans +=left;
        }
        return ans;
    }
}
```

和1358题目类似哈
