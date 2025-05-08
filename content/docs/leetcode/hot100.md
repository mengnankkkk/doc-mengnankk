---
title: Leetcode HOT100题
categories:
  - java
  
abbrlink: 2701
date: 2025.3.2
tags: 
   - java 
   - 数据结构
   - leetcode
top: true
---



# hash

## 1.[hash映射](https://leetcode.cn/problems/two-sum/solutions/6873/jie-suan-fa-1-liang-shu-zhi-he-by-guanpengchn/?envType=study-plan-v2&envId=top-100-liked)

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案，并且你不能使用两次相同的元素。

你可以按任意顺序返回答案。

wp:

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i< nums.length; i++) {
            if(map.containsKey(target - nums[i])) {
                return new int[] {map.get(target-nums[i]),i};
            }
            map.put(nums[i], i);
        }
        throw new IllegalArgumentException("No two sum solution");
    }
}

```

这里主要是用了map映射，

遍历数组 nums，i 为当前下标，每个值都判断map中是否存在 **target-nums[i]** 的 key 值
如果存在则找到了两个值，如果不存在则将当前的 (nums[i],i) 存入 map 中，继续遍历直到找到为止

## 2.[字母异位词分组](https://leetcode.cn/problems/group-anagrams/)

给你一个字符串数组，请你将 **字母异位词** 组合在一起。可以按任意顺序返回结果列表。

**字母异位词** 是由重新排列源单词的所有字母得到的一个新单词。

wp:

字母相同，但排列不同的字符串，排序后都一定是相同的。因为每种字母的个数都是相同的，那么排序后的字符串就一定是相同的。

所以我们直接进行排序

使用stream流来做

```java
class Solution2 {
public List<List<String>> getAnagrams(String[] strs) {
    return new ArrayList<>(Arrays.stream(strs).collect(Collectors.groupingBy(str -> Stream.of(str.split("")).sorted().collect(Collectors.joining()))).values());
}
}
```

str -> split -> stream -> sort -> join

## 3.[128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)

给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

请你设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

wp:

首先本题是不能排序来做的，因为排序的时间复杂度为O(*n*log*n*)

不符合题目的要求

对于 *nums* 中的元素 *x*，以 *x* 为起点，不断查找下一个数 *x*+1,*x*+2,⋯ 是否在 *nums* 中，并统计序列的长度。

```java
class Solution3{
    public int longestConsecutive(int[] nums){
        int ans = 0;
        Set<Integer> st = new HashSet<>();
        for(int num:nums){
            st.add(num);
        }
        for(int x:st){
            if(st.contains(x-1)){
                continue;
            }
            int y = x+1;
            while(st.contains(y)){
                y++;
            }
            ans = Math.max(ans,y-x);
        }
        return ans;
    }
}
```

主要使用了hashset来存储nums这个数组

主要的核心就是for循环

```java
for(int x:st){
            if(st.contains(x-1)){
                continue;
            }
            int y = x+1;
            while(st.contains(y)){
                y++;
            }
            ans = Math.max(ans,y-x);
        }
```

就是去试x周围的数据，x-1和x+1。最后取值y-x获得最大序列的长度

# 双指针

## 4.[283. 移动零](https://leetcode.cn/problems/move-zeroes/)

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**请注意** ，必须在不复制数组的情况下原地对数组进行操作。

wp:

这里我们参考快速排序的方法，以0为基准元素

我们使用两个指针 `i` 和 `j`，只要 `nums[i]!=0`，我们就交换 `nums[i]` 和 `nums[j]`在

这样我们很快就能完成排序

时间复杂度：*O*(*n*)

```java
class Solution4 {
    public void moveZeroes(int[] nums){
        if (nums == null){
            return;
        }
        int j =0;
        for (int i=0;i<nums.length;i++){
            if (nums[i]!=0){
                int tmp = nums[i];
                nums[i] = nums[j];
                nums[j++] = tmp;
            }
        }
    }
}
```

## 5.[11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)

给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

**说明：**你不能倾斜容器。

用一句话概括双指针解法的要点：**指针每一次移动，都意味着排除掉了一个柱子**。

如果固定左边的柱子，移动右边的柱子，那么水的高度一定不会增加，且宽度一定减少，所以水的面积一定减少。这个时候，左边的柱子和任意一个其他柱子的组合，其实都可以排除了。也就是我们可以排除掉左边的柱子了。

```java
class Solution5{
    public int maxArea(int[] height){
        int res = 0;
        int i = 0;
        int j = height.length-1;
        while (i<j){
            int area = (j-i)*Math.min(height[i],height[j]);
            res = Math.max(res,area);
            if (height[i]<height[j]){
                i++;
            }else {
                j--;
            }
        }
        return res;
    }
}
```

6.[三数之和](https://leetcode.cn/problems/3sum/)

给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请你返回所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

首先对数组进行排序

固定一个数i

L是i后面一个，R是len的最后一个

如果nums[i]大于0的话,sum必然大于0

如果num[i]==nums[i-1]的话，说明数字重复了，需要跳过

sum==0的时候，nums[L]==nums[L+1]重复舍去

nums[R] = nums[R-1]重负舍去

其中L是++的，R是——的

然后开始编码

当sum>0的时候说明R太大了，R--

Sum<0的时候说明L太小了，L++

wp:

```java
class Solution6{
    public static List<List<Integer>> threeSum(int[] nums){
        List<List<Integer>> ans = new ArrayList<>();
        int len = nums.length;
        if (nums==null||len<3) return ans;
        Arrays.sort(nums);
        for(int i =0;i<len;i++){
            if (nums[i]>0) break;
            if (i>0&&nums[i]==nums[i-1]) continue;
            int L = i+1;
            int R = len - 1;
            while (L<R){
                int sum = nums[i] + nums[L] + nums[R];
                if (sum == 0){
                    ans.add(Arrays.asList(nums[i],nums[L],nums[R]));
                    while (L<R&&nums[L] == nums[L+1]) L++;
                    while (L<R&&nums[R] == nums[R-1]) R--;
                    L++;
                    R--;
                }
                else if (sum<0) L++;
                else if (sum>0) R--;
            }
        }
        return ans;
    }
}
```

## 85.[接雨水](https://leetcode.cn/problems/trapping-rain-water/)

给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。



使用双指针，谁小谁移动规则下，相遇的一定是最高的位置，这个位置不能接水

```java
class Solution85{
    public int trap(int[] height){
        int ans  = 0;
        int left = 0;
        int right  = height.length-1;
        int preMax = 0;
        int sufMax=0;
        while (left<right){
            preMax = Math.max(preMax,height[left]);
            sufMax  = Math.max(sufMax,height[right]);
            ans +=preMax<sufMax?preMax-height[left++]:sufMax-height[right--];
        }
        return ans;
    }
}
```



# 滑动窗口

## 6.[无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长 子串** 的长度。

wp:

hashmap `dic`统计：指针j遍历字符s，哈希表统计s[j]最后一次出现的索引

根据上轮i与dis[s[j]]更新左边界i，保证[i+1,j]内无重复字符且最大

i= max(dic[s[j]],i)

更新res就是[i+1,j]的len即j-1的最大值

```java
class Solution7{
    public int lenOfLongesSubstring(String s){
        Map<Character,Integer> dic  = new HashMap<>();
        int i=-1,res = 0,len =s.length();
        for(int j=0;i<len;j++){
            if (dic.containsKey(s.charAt(j)))
                i = Math.max(i,dic.get(s.charAt(j)));
            dic.put(s.charAt(j),j);
            res = Math.max(res,j-i);
        }
        return res;
    }
}
```

## 41.[找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)

给定两个字符串 `s` 和 `p`，找到 `s` 中所有 `p` 的 **异位词** 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

 

```java
class Solution41{
    public List<Integer> findAnagrams(String s,String p) {
        List<Integer> ans = new ArrayList<>();
        int[] cnt = new int[26];
        for (char c : p.toCharArray()) {
            cnt[c - 'a']++;
        }
        int left = 0,right=0,required = p.length();
        while (right<s.length()){
            int c = s.charAt(right) -'a';
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
}
```

```
 int[] cnt = new int[26];
        for (char c : p.toCharArray()) {
            cnt[c - 'a']++;  
        }
```

来统计p中字符的次数

```
while (right < s.length()) {
            // 处理右侧新进入窗口的字符
            int c = s.charAt(right) - 'a';
            if (cnt[c] > 0) {
                required--;  // 需要匹配的字符减少
            }
            cnt[c]--;
            right++;  // 扩展窗口

​        // 当窗口大小等于 p.length()，检查是否是异位词
​        if (required == 0) {
​            ans.add(left);
​        }
```

然后超过了窗口

需要收缩窗口right - left == p.length()

```
 int l = s.charAt(left) - 'a';
                if (cnt[l] >= 0) {
                    required++;  // 需要匹配的字符增加
                }
                cnt[l]++;
                left++;  // 收缩窗口
```

跟上面想对应

最后返回ans

85.[滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。

返回 *滑动窗口中的最大值* 。

```java
class Solution86{
    public int[] maxSlidingWindow(int[] nums, int k ){
        if (nums.length==0||k==0) return new int[0];
        Deque<Integer> deque = new LinkedList<>();
        int[] res = new int[nums.length-k+1];
        for (int j =0,i=1-k;j<nums.length;i++,j++){
            if (i>0&&deque.peekFirst()==nums[i-1])
                deque.removeFirst();
            while (!deque.isEmpty()&&deque.peekLast()<nums[j])
                deque.removeLast();
            deque.addLast(nums[j]);
            if (i>=0)
                res[i] =deque.peekFirst();
            
        }
        return res;
    }

}
```

 具体是使用i和j这两个移动的指针来完成窗口的滑动

nums.length-k+1是窗口可以滑动的次数

形成首个窗口之前，一直都是队列中加入j

然后当i>=0了，这个时候形成了滑动窗口，队列中最大的那个元素就可以进入结果res了

然后继续往下遍历

遇到对了不为空，并且最后一个小于当前j元素的，把后边的元素移除，在后面加入j

让队列一直处于递减的状态

队头移出的时候，将头部删除，把过期元素删除



# 链表

## 7.[相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

给你两个单链表的头节点 `headA` 和 `headB` ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 `null` 。

```java
class Solution8 {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) return null;

        ListNode A = headA, B = headB;

        while (A != B) {
            A = (A != null) ? A.next : headB;
            B = (B != null) ? B.next : headA;
        }

        return A;
    }
}

class ListNode {
    int val;
    ListNode next;

    ListNode(int x) {
        val = x;
        next = null;
    }
}

```

先处理一下A与B不为空的情况

最重要的是

while (A != B) {
            A = (A != null) ? A.next : headB;
            B = (B != null) ? B.next : headA;
        }

两个具有相同结尾的链表拼接，无论哪一个在前，哪一个在后，这两种拼接方式，他们总能保持最后的一段相同的节点是不变的。

## 8.[反转链表](https://leetcode.cn/problems/reverse-linked-list/)

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

直接通过双指针反转

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode cur = head, pre = null;
        while(cur != null) {
            ListNode tmp = cur.next; // 暂存后继节点 cur.next
            cur.next = pre;          // 修改 next 引用指向
            pre = cur;               // pre 暂存 cur
            cur = tmp;               // cur 访问下一节点
        }
        return pre;
    }
}
```

9.[回文链表](https://leetcode.cn/problems/palindrome-linked-list/)

给你一个单链表的头节点 `head` ，请你判断该链表是否为。如果是，返回 `true` ；否则，返回 `false` 。

这个题是在上面的基础上进行更改

```java
class Solution9{
    public ListNode reverList(ListNode head){
        ListNode cur = head,pre  = null;
        while (cur!=null){
            ListNode tmp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = tmp;

        }
        return pre;
    }
    public boolean isPalindrome(ListNode head){
        ListNode mid =middleNode(head);
        ListNode head2  = reverList(mid);
        while (head2!=null){
            if (head.val!=head2.val){
                return false;
            }
            head = head.next;
            head2 = head2.next;
        }
        return true;
    }
    private ListNode middleNode(ListNode head){
        ListNode slow = head;
        ListNode fast = head;
        while (fast!=null&&fast.next!=null){
            slow = slow.next;
            fast = fast.next.next;
       }return slow;
    }
}
```

先找到中间的节点，也就是middleNode函数

slow比慢等到fast.next到终点的时候.slow就是中间的节点

然后反正mid那一部分，如果mid与反转后的相同的话，那么就是回文串

返回ture反之则false

## 9.环形链表1

给你一个链表的头节点 `head` ，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。**注意：`pos` 不作为参数进行传递** 。仅仅是为了标识链表的实际情况。

*如果链表中存在环* ，则返回 `true` 。 否则，返回 `false` 。



如果一个链表存在环，那么**快慢指针必然会相遇**。实现代码如下

所以我们直接编写代码

```java
class Solution10{
        public boolean hasCycle(ListNode head){
            ListNode slow = head,fast = head;
            while (fast!=null&&fast.next!=null){
                slow = slow.next;
                fast = fast.next.next;
                if (fast==slow){
                    return true;
                }
            }
            return false;
        }
    }
```

## 10.环形链表2

给定一个链表的头节点  `head` ，返回链表开始入环的第一个节点。 *如果链表无环，则返回 `null`。*

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（**索引从 0 开始**）。如果 `pos` 是 `-1`，则在该链表中没有环。**注意：`pos` 不作为参数进行传递**，仅仅是为了标识链表的实际情况。

**不允许修改** 链表。

根据分析，有环的标识是fast==slow

然后fast和slow第二次相遇的node就是环的节点

所以很简单的就可以分析出

```java
class Solution11{
        public ListNode detectCycle(ListNode head){
            ListNode fast = head,slwo = head;
            while (fast!=null&&fast.next!=null){
                fast = fast.next.next;
                slwo = slwo.next;
                if (fast == slwo) {
                    fast = head;
                    while (slwo!=fast){
                        slwo = slwo.next;
                        fast = fast.next;
                    }
                    return fast;
                }
            }
            return null;
        }
    }
```

直接使用两次相遇，相遇的fast就是环的起点；

## 11.[合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

 我们使用递归算法

当两个链表都为空的时候，说明已经合并完毕了

l1和l2哪个同头节点更小，较小节点的Next指针就指向其余节点的合并结果

```java
class Solution12{
        public ListNode mergeTowLists(ListNode l1,ListNode l2){
            if (l1==null){
                return l2;
            }
            else if (l2==null){
                return l1;
            }
            else if (l1.val<l2.val){
                l1.next = mergeTowLists(l1.next,l2);
                return l1;
            }
            else {
                l2.next = mergeTowLists(l1,l2.next);
                return l2;
            }
        }
    }
```

## 12.[两数相加](https://leetcode.cn/problems/add-two-numbers/)

给你两个 **非空** 的链表，表示两个非负的整数。它们每位数字都是按照 **逆序** 的方式存储的，并且每个节点只能存储 **一位** 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。‘



计算每一位的时候要考虑上一位的进位问题，计算结束后同样要跟更新进位

如果两个链表全部遍历完毕后，进位值为 1，则在新链表最前方添加节点 1

```java
class Solution13 {
        public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
            ListNode pre = new ListNode(0);
            ListNode cur = pre;
            int carry = 0;
            while (l1 != null || l2 != null) {
                int x = l1 == null ? 0 : l1.val;
                int y = l2 == null ? 0 : l2.val;
                int sum = x + y + carry;

                carry = sum / 10;
                sum = sum % 10;
                cur.next = new ListNode(sum);

                cur = cur.next;
                if (l1 != null) {
                    l1 = l1.next;
                }
                if (l2 != null) {
                    l2 = l2.next;
                }
            }
            if (carry == 1) {
                cur.next = new ListNode(carry);
            }
            return pre.next;
        }
    }
```

## 13.[删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

设预先指针 pre 的下一个节点指向 head，设前指针为 start，后指针为 end，二者都等于 pre
start 先向前移动n步
之后 start 和 end 共同向前移动，此时二者的距离为 n，当 start 到尾部时，end 的位置恰好为倒数第 n 个节点

因为要删除该节点，所以要移动到该节点的前一个才能删除，所以循环结束条件为 `start.next != null`

删除后返回 `pre.next`，为什么不直接返回 `head` 呢，因为 `head` 有可能是被删掉的点

```java
class Solution14 {
            public ListNode removeNthFromEnd(ListNode head, int n) {
                ListNode pre = new ListNode(0);
                pre.next = head;
                ListNode start = pre, end = pre;
                while (n != 0) {
                    start = start.next;
                    n--;
                }//start先提前移动
                while (start.next != null) {
                    start = start.next;
                    end = end.next;
                }//一块移动
                end.next = end.next.next;//删除某节点
                return pre.next;
            }
        }
```

## 14.[两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)

给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

调用单元：设需要交换的两个点为 head 和 next，head 连接后面交换完成的子链表，next 连接 head，完成交换
终止条件：head 为空指针或者 next 为空指针，也就是当前无节点或者只有一个节点，无法进行交换

假设链表为

```
tmp -> (A) -> (B) -> (C) -> (D) -> null
start -> (B)
end -> (C)

```

使tmp.next = end;

链表现在变成了这样

```
(A) -> (C) -> (D) -> null
(B) (断开)

```

再start.next = end.next;

(B)就指向了（D）

end.next = start;

然后（C）就指向了（B）

所以链表就变成了

```
(A) -> (C) -> (B) -> (D) -> null
```

最后tmp = start;

再从B开始继续迭代循环

最终完成了链表节点的交换

完整代码：



```java
class Solution15 {
    public ListNode swapPairs1(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }//没有节点，或者只剩一个的时候
        ListNode next = head.next;
        head.next = swapPairs1(next.next);//
        next.next = head;//后节点等于头节点
        return next;
    }
    public ListNode swapPairs2(ListNode head){
        ListNode pre = new ListNode(0);
        pre.next = head;
        ListNode tmp = pre;
        while (tmp.next!=null&&tmp.next.next!=null){
            ListNode start = tmp.next;
            ListNode end = tmp.next.next;
            tmp.next = end;//head
            start.next = end.next;
            end.next = start;
            tmp = start;
        }
        return pre.next;
    }

}
```

## 15.[ 随机链表的复制](https://leetcode.cn/problems/copy-list-with-random-pointer/)

给你一个长度为 `n` 的链表，每个节点包含一个额外增加的随机指针 `random` ，该指针可以指向链表中的任何节点或空节点。

构造这个链表的 **[深拷贝](https://baike.baidu.com/item/深拷贝/22785317?fr=aladdin)**。 深拷贝应该正好由 `n` 个 **全新** 节点组成，其中每个新节点的值都设为其对应的原节点的值。新节点的 `next` 指针和 `random` 指针也都应指向复制链表中的新节点，并使原链表和复制链表中的这些指针能够表示相同的链表状态。**复制链表中的指针都不应指向原链表中的节点** 。

例如，如果原链表中有 `X` 和 `Y` 两个节点，其中 `X.random --> Y` 。那么在复制链表中对应的两个节点 `x` 和 `y` ，同样有 `x.random --> y` 。

返回复制链表的头节点。

用一个由 `n` 个节点组成的链表来表示输入/输出中的链表。每个节点用一个 `[val, random_index]` 表示：

- `val`：一个表示 `Node.val` 的整数。
- `random_index`：随机指针指向的节点索引（范围从 `0` 到 `n-1`）；如果不指向任何节点，则为 `null` 。

你的代码 **只** 接受原链表的头节点 `head` 作为传入参数。

```java
class Solution16{
    public Node copyRandomList(Node head){
        if (head==null) return null;
        Node cur =head;
        Map<Node,Node> map = new HashMap<>();
        while (cur!=null){
            map.put(cur,new Node(cur.val));
            cur = cur.next;
        }
        cur = head;
        while (cur!=null){
            map.get(cur).next = map.get(cur.next);
            map.get(cur).random = map.get(cur.random);
            cur = cur.next;
        }
        return map.get(head);
    }
}
```

while (cur!=null){
            map.put(cur,new Node(cur.val));
            cur = cur.next;
        }

先把链表复制一份

构建新节点的 `next` 和 `random` 引用指向。都是随机的

然后迭代下一个节点

最后返回head节点

## 16.[排序链表](https://leetcode.cn/problems/sort-list/)

给你链表的头结点 `head` ，请将其按 **升序** 排列并返回 **排序后的链表** 。

我们使用新建链表的方式

把val值排序后再组成一个新链表

```java
class Solution17{
    public ListNode sortList(ListNode head){
        if (head==null){
            return null;
        }
        ListNode cur = head;
        int n = 0;
        while (cur!=null){
            n++;
            cur = cur.next;
        }
        int[] arr = new int [n];
        cur = head;
        for(int i=0;i<arr.length;i++){
            arr[i] = cur.val;
            cur = cur.next;
        }
        Arrays.sort(arr);
        ListNode listNode = new ListNode(arr[0]);
        cur = listNode;
        for (int i=1;i<arr.length;i++){
            cur.next = new ListNode(arr[i]);
            cur = cur.next;
        }
        return listNode;
    }
}

```

## 17.[LRU 缓存](https://leetcode.cn/problems/lru-cache/)

请你设计并实现一个满足 [LRU (最近最少使用) 缓存](https://baike.baidu.com/item/LRU) 约束的数据结构。

实现 `LRUCache` 类：

- `LRUCache(int capacity)` 以 **正整数** 作为容量 `capacity` 初始化 LRU 缓存
- `int get(int key)` 如果关键字 `key` 存在于缓存中，则返回关键字的值，否则返回 `-1` 。
- `void put(int key, int value)` 如果关键字 `key` 已经存在，则变更其数据值 `value` ；如果不存在，则向缓存中插入该组 `key-value` 。如果插入操作导致关键字数量超过 `capacity` ，则应该 **逐出** 最久未使用的关键字。

函数 `get` 和 `put` 必须以 `O(1)` 的平均时间复杂度运行。

基于LinkedHashMap来实现

`LinkedHashMap` 是 **有序哈希表**，可以按**插入顺序**或**访问顺序**存储键值对。

super(capacity, 0.75F, true);

**capacity**: 初始容量

**0.75F**: 负载因子（默认 `0.75`）

**true**: 启用 **访问顺序**，即 **最近访问的元素会被移到链表尾部**，最久未使用的元素会在链表头部。

`size() > capacity` 时，返回 `true`，`LinkedHashMap` 会自动删除 **链表头部的最老元素**（即最近最少使用的元素）。

```java
class LRUCache extends LinkedHashMap<Integer,Integer> {
    private int capacity;
    public LRUCache(int capacity) {
        super(capacity,0.75F,true);
        this.capacity = capacity;
    }
    public int get(int key) {
        return super.getOrDefault(key,-1);
    }
    public void put(int key, int value) {
        super.put(key,value);
    }
    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer,Integer> eldest){
        return size() > capacity;
    }
}
```

# 数组

## 18.[最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组**是数组中的一个连续部分。

 

```java
class Solution18{
    public int maxSubArray(int[] nums){
        int ans = nums[0];
        int sum = 0;
        for (int num:nums){
            if (sum>0){
                sum+=num;
            }
            else {
                sum = num;
            }
            ans=Math.max(ans,sum);
        }
        return ans;
    }
}
```

直接使用循环来完成这个目的

## 19.[合并区间](https://leetcode.cn/problems/merge-intervals/)

以数组 `intervals` 表示若干个区间的集合，其中单个区间为 `intervals[i] = [starti, endi]` 。请你合并所有重叠的区间，并返回 *一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间* 。

1. 把 *intervals*[0] 加入答案。注意，答案的最后一个区间表示**当前正在合并的区间**。

2. 遍历到 intervals[1]=[2,6]，由于左端点 2 不超过当前合并区间的右端点 3，可以合并。由于右端点 6>3，那么更新当前合并区间的右端点为 6。注意，由于我们已经按照左端点排序，所以 intervals[1] 的左端点 2 必然大于等于合并区间的左端点，所以无需更新当前合并区间的左端点。

3. 遍历到 intervals[2]=[8,10]，由于左端点 8 大于当前合并区间的右端点 6，无法合并（两个区间不相交）。再次利用区间按照左端点排序的性质，更后面的区间的左端点也大于 6，无法与当前合并区间相交，所以当前合并区间 [1,6] 就固定下来了，把新的合并区间 [8,10] 加入答案。

4. 遍历到 intervals[3]=[15,18]，由于左端点 15 大于当前合并区间的右端点 10，无法合并（两个区间不相交），我们找到了一个新的合并区间 [15,18] 加入答案。

   上述算法同时说明，按照左端点排序后，合并的区间一定是 *intervals* 中的连续子数组。

```java
class Solution19{
    public int[][] merge(int[][] intervals){
        Arrays.sort(intervals,(p,q)->p[0] - q[0]);//按照左端点从小到大排序
        List<int[]> ans = new ArrayList<>();
        for (int[] p:intervals){
            int m = ans.size();
            if (m>0&&p[0]<=ans.get(m-1)[1]){//限制范围
                ans.get(m-1)[1] = Math.max(ans.get(m-1)[1],p[1]);
            }
            else{
                ans.add(p);
            }
        }
        return ans.toArray(new int[ans.size()][]);

    }
}
```

## 20.[轮转数组](https://leetcode.cn/problems/rotate-array/)

给定一个整数数组 `nums`，将数组中的元素向右轮转 `k` 个位置，其中 `k` 是非负数。

```java
class Solution{
    public void rotate(int[] nums,int k){
        k%=nums.length;
        reverse(nums,0,nums.length-1);
        reverse(nums,0,k-1);
        reverse(nums,k,nums.length-1);
        
    }
    public void reverse(int[] nums,int start,int end){
        while (start<end){
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = nums[temp];
            start+=1;
            end-=1;
        }
    }
}
```

这个主要是轮转，

首先对整个数组实行翻转，这样子原数组中需要翻转的子数组，就会跑到数组最前面。

这时候，从 *k* 处分隔数组，左右两数组，各自进行翻转即可。

然后k逐渐求余

然后就构建一个反转函数就行了

## 21.[除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/)(x)

给你一个整数数组 `nums`，返回 数组 `answer` ，其中 `answer[i]` 等于 `nums` 中除 `nums[i]` 之外其余各元素的乘积 。

题目数据 **保证** 数组 `nums`之中任意元素的全部前缀元素和后缀的乘积都在 **32 位** 整数范围内。

请 **不要使用除法，**且在 `O(n)` 时间复杂度内完成此题。

 



```java
class Solution {
        //假如nums为[1,2,3,4]，那么answer的值分别为[(2,3,4),(1,3,4),(1,2,4),(1,2,3)]
        //如果吧i当前值相乘的时候看做是1那么就有如下样式
        //  1, 2, 3, 4 
        //  1, 1, 3, 4
        //  1, 2, 1, 4
        //  1, 2, 3, 1
        // 他的对角线1将他们分割成了两个三角形，对于answer的元素，
        //我们可以先计算一个三角形每行的乘积，然后再去计算另外一个三角形每行的乘积，
        //然后各行相乘，就是answer每个对应的元素
    public int[] productExceptSelf(int[] nums) {
        int length = nums.length;
        
        //先初始化一个answer数组,但是很多解答都没说明的是这个answer数组，
        //并不是以此计算就得出的结果,而是两次乘积之后的结果
        int[] answer = new int[length];
        //初始化一个初始值，作为三角乘积计算的开始
        answer[0] = 1;
        //先计算左边三角的乘积
        for(int i = 1; i < length; i++){
            answer[i] = answer[i-1] * nums[i-1];
        }
        //再次计算右边三角形,为什么是length-2呢？
        //length-1是最后一个值的索引，但是最后一个值temp[length-1] = 1,
        //也是对应对角线上的1，所以不在进行相乘处理
        //temp的作用是计算右边三角形的乘积的累计值，然后再和answer相乘，
        //注意!!!:不能直接nums[i+1]相乘那会在计算右三角的时候变成每行乘积与nums[i+1]的错误答案
        int temp = 1;
        for(int i = length - 2; i >= 0; i--){
            //先将每行乘积赋予一个中间值
            temp *= nums[i+1];
            answer[i] *= temp;
        }
        return answer;
    }
}
```

## 87.[缺失的第一个正数](https://leetcode.cn/problems/first-missing-positive/)

给你一个未排序的整数数组 `nums` ，请你找出其中没有出现的最小的正整数。

请你实现时间复杂度为 `O(n)` 并且只使用常数级别额外空间的解决方案。

```java
class Solution87{
    public int firstMissingPositive(int[] nums){
        int n = nums.length;
        Set<Integer> set  =new HashSet<>();
        int min = Integer.MAX_VALUE;
        for (int num:nums){
            if (num<min){
                min = num;
            }
            set.add(num);

        }//找到最小值
        if (min>1&&!set.contains(1)) return 1;
        while (set.contains(min)){
            min++;
            if (min<0) min = 1;
            if (set.contains(min)) min++;
            else {
                if (min<=0) min++;
            }//将负数Min++直到为1
        }
        return min>0?min:1;//不在集合中，大于0直接返回
    }
}
```

就是把负数变为1

其余的返回

# 树

## 22.[二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

给定一个二叉树的根节点 `root` ，返回 *它的 **中序** 遍历* 。

 中序遍历是左->根->右

可以使用递归完成dfs的中序遍历

```java
class Solution22{
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        dfs(res, root);
        return res;
    }
        void dfs(List<Integer> res,TreeNode root){
            if (root==null){
                return ;
            }
            dfs(res,root.left);
            res.add(root.val);
            dfs(res,root.right);
    }
}
```

## 23.[二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

给定一个二叉树 `root` ，返回其最大深度。

二叉树的 **最大深度** 是指从根节点到最远叶子节点的最长路径上的节点数。

```java
class Solution23{
    public int maxDepth(TreeNode root){
        if (root==null){
            return 0;
        }
        else {
            int left = maxDepth(root.left);
            int right = maxDepth(root.right);
            return Math.max(left,right)+1;
        }
    }
}
```

就是左子树和右子树分别往下找，然后他们两个最大的+1（root）就是最大深度

## 24.[翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

给你一棵二叉树的根节点 `root` ，翻转这棵二叉树，并返回其根节点。

```java
class Solution24{
    public TreeNode invertTree(TreeNode root){
        if (root==null) return null;
        TreeNode tmp = root.left;
        root.left = invertTree(root.right);
        root.right = invertTree(tmp);
        return root;
    }
}
```

就是左子树变成右子树，右子树变成左子树

直接设置一个中间值，然后直接递归交换即可。

## 25.[对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

给你一个二叉树的根节点 `root` ， 检查它是否轴对称。

```java
class Solution25{
    public boolean isSymmetric(TreeNode root){
        return root==null||recur(root.left,root.right);
    }
    boolean recur(TreeNode L,TreeNode R){
        if (L==null&&R==null) return true;
        if (L==null||R==null||L.val!=R.val) return false;
        return recur(L.left,R.right)&&recur(L.right,R.left);
    }
}
```

还是递归的方法，如果左空右空，那么就肯定对称啊

左不为空或者右不为空，val还不一样肯定不是对称

然后递归往下顺延

左的左，对应右的右。左的右对应右的左。然后递归往下找下一个节点

## 26.[二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/)

给你一棵二叉树的根节点，返回该树的 **直径** 。

二叉树的 **直径** 是指树中任意两个节点之间最长路径的 **长度** 。这条路径可能经过也可能不经过根节点 `root` 。

两节点之间路径的 **长度** 由它们之间边数表示。

```java
class Solution26{
    private int ans;
    
    public int diameterOfBinaryTree(TreeNode root){
        dfs(root);
        return ans;
    }
    private int dfs(TreeNode node){
        if (node ==null){
            return -1;
        }
        int llen = dfs(node.left)+1;
        int rlen = dfs(node.right)+1;
        ans = Math.max(ans,llen+rlen);
        return Math.max(llen,rlen);
    }
}
```

找到左子树的最大长，和右子树的最大长，加起来就是长度。

其中+1是经过root节点

## 27.[二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

给你二叉树的根节点 `root` ，返回其节点值的 **层序遍历** 。 （即逐层地，从左到右访问所有节点）。

 

使用递归的方法

```java
class Solution27{
    public List<List<Integer>> levelOrder(TreeNode root){
        List<List<Integer>> res = new ArrayList<>();
        if (root==null) return res;
        Queue<TreeNode> que = new LinkedList<>();
        que.add(root);
        while (!que.isEmpty()){
            int sized = que.size();
            List<Integer> layer = new ArrayList<>();
            while (sized-->0){
                TreeNode poll = que.poll();
                layer.add(poll.val);
                if (poll.left!=null) que.add(poll.left);
                if (poll.right!=null) que.add(poll.right);
            }
            res.add(layer);
        }return res;
    }
}
```

while (sized-->0){
                TreeNode poll = que.poll();
                layer.add(poll.val);
                if (poll.left!=null) que.add(poll.left);
                if (poll.right!=null) que.add(poll.right);
            }

sized逐渐递减，大于0的时候执行代码

```
TreeNode poll = que.poll();
                layer.add(poll.val);
                if (poll.left!=null) que.add(poll.left);
                if (poll.right!=null) que.add(poll.right);
```

poll为队列弹出的

然后List加入弹出的值

然后先向左递归，然后再向右递归

直到完成que为空，最后返回res

## 28.[将有序数组转换为二叉搜索树](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/)

给你一个整数数组 `nums` ，其中元素已经按 **升序** 排列，请你将其转换为一棵 平衡 二叉搜索树。

```java
class Solution28{
    public TreeNode sortedArrayToBST(int[] nums){
        return dfs(nums,0,nums.length-1);
    }
    private TreeNode dfs(int[] nums,int lo,int hi){
        if (lo>hi){
            return null;
        }
        int mid = lo+(hi-lo)/2;
        TreeNode root  = new TreeNode(nums[mid]);
        root.left = dfs(nums,lo,mid-1);
        root.right = dfs(nums,mid+1,hi);
        return root;
    }
}
```

使用了二分查找，主要是设定了mid为lo加上hi-lo的位置，这个位置就是root

然后递归查找，就可以出现二叉搜索树了。

二叉搜索树就是二分查找中出现的



## 29.[验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)

给你一个二叉树的根节点 `root` ，判断其是否是一个有效的二叉搜索树。

**有效** 二叉搜索树定义如下：

- 节点的左子树只包含 **小于** 当前节点的数。
- 节点的右子树只包含 **大于** 当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树

```java
class Solution29{
    public boolean isValidBST(TreeNode root){
        return isValidBST(root,Long.MIN_VALUE,Long.MAX_VALUE);
    }
    private boolean isValidBST(TreeNode node,long left,long right){
        if (node==null){
            return true;
        }
        long x = node.val;
        return left<x&&x<right&&
                isValidBST(node.left,left,x)&&
                isValidBST(node.right,x,right);
    }
}
```

二叉搜索树的特点是左子树是小于根节点的

右子树是大于根节点的

我们就根据这个来判断就行

return left<x&&x<right&&
                isValidBST(node.left,left,x)&&
                isValidBST(node.right,x,right);

这就是成立条件，其中用到了递归

## 30.[ 二叉搜索树中第 K 小的元素](https://leetcode.cn/problems/kth-smallest-element-in-a-bst/)（topk）

给定一个二叉搜索树的根节点 `root` ，和一个整数 `k` ，请你设计一个算法查找其中第 `k` 小的元素（从 1 开始计数）。

```java
class Solution30{
    int res,k;
    void dfs(TreeNode root){
        if (root==null) return;
        dfs(root.left);
        if (k==0) return;
        if (--k==0) res = root.val;
        dfs(root.right);
    }
    public int kthSmallest(TreeNode root,int k){
        this.k = k;
        dfs(root);
        return res;
    }
}
```

因为是找第K小的数，所以先从左子树开始

然后如果递归下去--k==0了说明不在左子树，是根节点

然后递归右子树

## 31.[二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/)

给定一个二叉树的 **根节点** `root`，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

```java
class Solution31{
    public List<Integer> rightSideView(TreeNode  root){
        List<Integer> ans = new ArrayList<>();
        dfs(root,0,ans);
        return ans;
    }
    private void dfs(TreeNode root,int depth,List<Integer> ans){
        if (root==null){
            return;
        }
        if (depth==ans.size()){
            ans.add(root.val);
        }
        dfs(root.right,depth+1,ans);
        dfs(root.left,depth+1,ans);
    }
}
```

因为是右视图，所以是从右边开始

如果深度首次遇到，说明是遇到了最右边的，把值收入

先递归右子树，保证首次遇到的一定是最右边的节点

然后逐渐递归，最先右边的递归

## 32.[ 二叉树展开为链表](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/)

给你二叉树的根结点 `root` ，请你将它展开为一个单链表：

- 展开后的单链表应该同样使用 `TreeNode` ，其中 `right` 子指针指向链表中下一个结点，而左子指针始终为 `null` 。
- 展开后的单链表应该与二叉树 [**先序遍历**](https://baike.baidu.com/item/先序遍历/6442839?fr=aladdin) 顺序相同。

```java
class Solution32 {
    public void flatten(TreeNode root) {
        while (root != null) {
            if (root.left != null) {
                TreeNode pre = root.left;
                while (pre.right != null) { // 找到左子树的最右节点
                    pre = pre.right;
                }
                pre.right = root.right; // 右子树接到左子树的最右节点上
                root.right = root.left; // 左子树变成右子树
                root.left = null; // 断开左子树
            }
            root = root.right; // 继续处理下一个节点
        }
    }
}
```

## 33.[路径总和 III](https://leetcode.cn/problems/path-sum-iii/)

给定一个二叉树的根节点 `root` ，和一个整数 `targetSum` ，求该二叉树里节点值之和等于 `targetSum` 的 **路径** 的数目。

**路径** 不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

```java
class Solution33{
    private int ans;
    public int pathSum(TreeNode root, int targetSum) {
        Map<Long, Integer> cnt = new HashMap<>();
        cnt.put(0L, 1);
        dfs(root, 0, targetSum, cnt);
        return ans;
    }
    private void dfs(TreeNode node,long s,int targetsum,Map<Long,Integer> cnt){
        if (node==null){
            return;
        }
        s+= node.val;
        ans+=cnt.getOrDefault(s-targetsum,0);
        cnt.merge(s,1,Integer::sum);//cnt[s++]
        dfs(node.left,s,targetsum,cnt);
        dfs(node.right,s,targetsum,cnt);
        cnt.merge(s,-1,Integer::sum);//归零
    }
}

```

 cnt.merge(s,1,Integer::sum);//想当与逐渐+1

然后走的时候先走左再走右

最后清零sum

## 34.[二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

[百度百科](https://baike.baidu.com/item/最近公共祖先/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

```java
class Solution34{
    public TreeNode lowestCommonAncestor(TreeNode root,TreeNode p,TreeNode q){
        if (root==null||root==p||root==q) return root;
        TreeNode left = lowestCommonAncestor(root.left,p,q);
        TreeNode right = lowestCommonAncestor(root.right,p,q);
        if (left==null) return right;
        if (right==null) return left;
        return root;
    }
}
```

如果两个节点有一个跟根节点相同的话，那么最近公共祖先就是root

然后继续递归，当到最下的时候，为Null就是上一个节点

## 35.[从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

给定两个整数数组 `preorder` 和 `inorder` ，其中 `preorder` 是二叉树的**先序遍历**， `inorder` 是同一棵树的**中序遍历**，请构造二叉树并返回其根节点。

 

```java
    int pre = 0;
    int in=0;
    public TreeNode buildTree2(int[] preorder,int[] inorder){
        return my(preorder,inorder,Integer.MAX_VALUE);
    }
    public TreeNode my(int[] preorder,int[] inorder,int stop){
        if (pre == preorder.length){
            return null;
        }
        if (inorder[in] ==stop){
            in++;
            return null;
        }
        TreeNode root = new TreeNode(preorder[pre++]);
        root.left = my(preorder,inorder,root.val);
        root.right = my(preorder,inorder,stop);
        return root;
    }
```

TreeNode root = new TreeNode(preorder[pre++]);逐渐获得前序遍历的值

然后构造左子树，构造右子树

没有子树的时候，返回in++，直到到stop

然后调用递归

# 二分查找

## 36.[搜索插入位置](https://leetcode.cn/problems/search-insert-position/)

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 `O(log n)` 的算法。

简单的二分查找，不比多说

```java
class Solution36{
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

## 37.[搜索二维矩阵](https://leetcode.cn/problems/search-a-2d-matrix/)

给你一个满足下述两条属性的 `m x n` 整数矩阵：

- 每行中的整数从左到右按非严格递增顺序排列。
- 每行的第一个整数大于前一行的最后一个整数。

给你一个整数 `target` ，如果 `target` 在矩阵中，返回 `true` ；否则，返回 `false` 。

 简单的矩阵二分查找

```java
class Solution37{
    public boolean searchMatrix(int[][] matrix,int target){
        int m = matrix.length;
        int n = matrix[0].length;
        int left = -1;
        int right = m*n;
        while (left+1<right){
            int mid =(left+right)>>>1;//计算中位数，即使为负数
            int x = matrix[mid/n][mid%n];
            if (x==target){
                return true;
            }
            if (x<target){
                left = mid;
            }
            else {
                right = mid;
            }
        }
        return false;
    }
}
```

*a*[*i*]=*matrix*[i/n】[imod*n*]

将矩阵转化为一个数组

## 38.[在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)

给你一个按照非递减顺序排列的整数数组 `nums`，和一个目标值 `target`。请你找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 `target`，返回 `[-1, -1]`。

你必须设计并实现时间复杂度为 `O(log n)` 的算法解决此问题。

```java
class Solution38{
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

首先是调用使用的方法然后如果超过了范围的话，返回-1,-1

如果成功的话返回start,end

要想找到 ≤target 的最后一个数，无需单独再写一个二分。我们可以先找到这个数的右边相邻数字，也就是 >target 的第一个数。在所有数都是整数的前提下，>target 等价于 ≥target+1，这样就可以复用我们已经写好的二分函数了，即 lowerBound(nums, target + 1)，算出这个数的下标后，将其减一，就得到 ≤target 的最后一个数的下标。

然后开始写二分查找的方法

## 39.[搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/)

整数数组 `nums` 按升序排列，数组中的值 **互不相同** 。

在传递给函数之前，`nums` 在预先未知的某个下标 `k`（`0 <= k < nums.length`）上进行了 **旋转**，使数组变为 `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`（下标 **从 0 开始** 计数）。例如， `[0,1,2,4,5,6,7]` 在下标 `3` 处经旋转后可能变为 `[4,5,6,7,0,1,2]` 。

给你 **旋转后** 的数组 `nums` 和一个整数 `target` ，如果 `nums` 中存在这个目标值 `target` ，则返回它的下标，否则返回 `-1` 。

你必须设计一个时间复杂度为 `O(log n)` 的算法解决此问题。

```java
class Solution39{
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

直接写出二分查找就可以了；

## 40.[寻找旋转排序数组中的最小值](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/)

已知一个长度为 `n` 的数组，预先按照升序排列，经由 `1` 到 `n` 次 **旋转** 后，得到输入数组。例如，原数组 `nums = [0,1,2,4,5,6,7]` 在变化后可能得到：

- 若旋转 `4` 次，则可以得到 `[4,5,6,7,0,1,2]`
- 若旋转 `7` 次，则可以得到 `[0,1,2,4,5,6,7]`

注意，数组 `[a[0], a[1], a[2], ..., a[n-1]]` **旋转一次** 的结果为数组 `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]` 。

给你一个元素值 **互不相同** 的数组 `nums` ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 **最小元素** 。

你必须设计一个时间复杂度为 `O(log n)` 的算法解决此问题。

```
class Solution40{
    public int findMin(int[] nums){
        int n = nums.length;
        int left  = -1;
        int right = n-1;
        while (left+1<right){
            int mid = (left+right)>>>1;
            if (nums[mid]<nums[n-1]){
                right = mid;
            }
            else {
                left = mid;
            }
        }
        return nums[right];
    }
}
```

就是简单的二分查找

只不过如果 nums[n−1] 是数组最小值，那么 nums 分成两段，第一段 [0,n−2]，第二段 [n−1,n−1]，且第一段的所有数都大于 nums[n−1]。每次 x 和 nums[n−1] 比大小，一定是 x>nums[n−1]。这意味着每次二分更新的都是 left，那么循环结束后，答案自然就是 n−1 了。。

# 矩阵

## 42.[矩阵置零](https://leetcode.cn/problems/set-matrix-zeroes/)

给定一个 `*m* x *n*` 的矩阵，如果一个元素为 **0** ，则将其所在行和列的所有元素都设为 **0** 。请使用 **[原地](http://baike.baidu.com/item/原地算法)** 算法**。**

```java
class Solution42{
    public void setZeroes(int[][] matrix){
        Set<Integer> row_zero = new HashSet<>();
        Set<Integer> col_zero = new HashSet<>();
        int row = matrix.length;
        int col = matrix[0].length;
        for (int i =0;i<row;i++){
            for (int j =0;j<col;j++){
                if (matrix[i][j]==0){
                    row_zero.add(i);
                    col_zero.add(j);
                }
            }
        }
        for (int i =0;i<row;i++){
            for (int j =0;j<col;j++){
                if (row_zero.contains(i)||col_zero.contains(j)){
                    matrix[i][j] = 0;
                }
            }
        }
    }
}
```

直接使用两层for循环进行遍历，然后将遇到的0的行和列的index放入

hashset

最后将hashset里的索引的值设定为0

## 43.[螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/)

给你一个 `m` 行 `n` 列的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。

```java
class Solution43 {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new ArrayList<>();
        if (matrix.length == 0) return res;

        int l = 0, r = matrix[0].length - 1;
        int t = 0, b = matrix.length - 1;

        while (l <= r && t <= b) {
            // 从左到右
            for (int i = l; i <= r; i++) res.add(matrix[t][i]);
            t++;  // 更新上边界
            if (t > b) break;

            // 从上到下
            for (int i = t; i <= b; i++) res.add(matrix[i][r]);
            r--;  // 更新右边界
            if (l > r) break;

            // 从右到左
            for (int i = r; i >= l; i--) res.add(matrix[b][i]);
            b--;  // 更新下边界
            if (t > b) break;

            // 从下到上
            for (int i = b; i >= t; i--) res.add(matrix[i][l]);
            l++;  // 更新左边界
            if (l > r) break;
        }

        return res;
    }
}
```

螺旋就是从左到右，然后从上到下，从右到左，再从下到上

l为左边界

t为上边界

r为右边界

b为下边界

然后依次for循环遍历即可

## 44.[旋转图像](https://leetcode.cn/problems/rotate-image/)

给定一个 *n* × *n* 的二维矩阵 `matrix` 表示一个图像。请你将图像顺时针旋转 90 度。

你必须在**[ 原地](https://baike.baidu.com/item/原地算法)** 旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要** 使用另一个矩阵来旋转图像。

```
class Solution44{
    public void rotate(int[][] matrix){
        int n = matrix.length;
        int[][] matrix_new  = new int[n][n];
        for (int i =0;i<n;++i){
            for (int j =0;j<n;j++){
                matrix_new[j][n-i-1] = matrix_new[i][j];
            }
        }
        for (int i=0;i<n;++i){
            for (int j=0;j<n;++j){
                matrix[i][j] = matrix_new[i][j];
            }
        }
    }
}

```

主要是新建一个矩阵，然后将旧的一一对应过去。

对于矩阵中第 *i* 行的第 *j* 个元素，在旋转后，它出现在倒数第 *i* 列的第 *j* 个位置。

```
matrix_new[j][n-i-1] = matrix_new[i][j];
```

## 45.[搜索二维矩阵 II](https://leetcode.cn/problems/search-a-2d-matrix-ii/)

编写一个高效的算法来搜索 `*m* x *n*` 矩阵 `matrix` 中的一个目标值 `target` 。该矩阵具有以下特性：

- 每行的元素从左到右升序排列。
- 每列的元素从上到下升序排列。

从左下角开始遍历

ij大于目标值，i--，则往上去找

ij小于目标值，j++，则往右去找

这样就是一个搜索

知道找到target为止

```java
class Solution45{
    public boolean searchMatrix(int[][] matrix,int target){
        int i = matrix.length-1,j=0;
        while (i>=0&&j<matrix[0].length){
            if (matrix[i][j]>target) i--;
            else if (matrix[i][j]<target) j++;
            else return true;
        }
        return false;
    }
}
```

# 字串

## 46.[和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/)

给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回 *该数组中和为 `k` 的子数组的个数* 。

子数组是数组中元素的连续非空序列。

```java
class Solution46{
    public int subarraySum(int[] nums,int k){
        int n = nums.length;
        int [] s = new int [n+1];
        for (int i =0;i<n;i++){
            s[i+1] = s[i] + nums[i];
        }
        int ans= 0;
        Map<Integer,Integer> cnt = new HashMap<>(n+1);
        for (int sj:s){
            ans+=cnt.getOrDefault(sj-k,0);
            cnt.merge(sj,1,Integer::sum);//cnt[sj]++
        }
        return ans;
    }
}

```

这个题先逐渐累加和存入s[]里面,这就是前缀和

`cnt` 用来存储 **某个前缀和出现的次数**，

cnt.getOrDefault(sj - k, 0)` 统计符合条件的 `s[i]` 个数，并累加到 `ans

# 图

## 47.[岛屿数量](https://leetcode.cn/problems/number-of-islands/)

给你一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围

```java
class Solution47{
    public int numIslands(char[][] grid){
        int count = 0;
        for (int i =0;i<grid.length;i++){
            for (int j=0;j<grid[0].length;j++){
                if (grid[i][j] =='1'){
                    dfs(grid,i,j);
                    count++;
                }
            }
        }
        return count;
    }
    private void dfs(char[][] grid,int i,int j){
        if (i<0||j<0||i>=grid.length||j>=grid[0].length||grid[i][j]=='0') return;
        grid[i][j]='0';
        dfs(grid,i+1,j);
        dfs(grid,i,j+1);
        dfs(grid,i-1,j);
        dfs(grid,i,j-1);

    }
}

```

直接dfs 深度优先遍历即可

## 48.[腐烂的橘子](https://leetcode.cn/problems/rotting-oranges/)(x)

在给定的 `m x n` 网格 `grid` 中，每个单元格可以有以下三个值之一：

- 值 `0` 代表空单元格；
- 值 `1` 代表新鲜橘子；
- 值 `2` 代表腐烂的橘子。

每分钟，腐烂的橘子 **周围 4 个方向上相邻** 的新鲜橘子都会腐烂。

返回 *直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 `-1`* 。

```java
class Solution {
    public int orangesRotting(int[][] grid) {
        int[][] dir = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
        int m = grid.length;
        int n = grid[0].length;
        Queue<int[]> queue = new LinkedList<>();
        int freshCount = 0;

        // 统计新鲜橘子数量，并找到腐烂橘子的初始位置
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {  // 修正错误 j < 0
                if (grid[i][j] == 2) {
                    queue.offer(new int[]{i, j});
                } else if (grid[i][j] == 1) {
                    freshCount++;
                }
            }
        }

        // 没有新鲜橘子，直接返回 0
        if (freshCount == 0) {
            return 0;
        }

        int time = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            boolean hasRotten = false;
            for (int i = 0; i < size; i++) {
                int[] arr = queue.poll();
                int x = arr[0];
                int y = arr[1];

                // 遍历 4 个方向
                for (int j = 0; j < 4; j++) {
                    int xNext = x + dir[j][0];
                    int yNext = y + dir[j][1];  // 修正错误：应使用 dir[j][1]

                    // 检查边界条件 & 是否是新鲜橘子
                    if (xNext >= 0 && yNext >= 0 && xNext < m && yNext < n && grid[xNext][yNext] == 1) {
                        grid[xNext][yNext] = 2;
                        queue.offer(new int[]{xNext, yNext});
                        freshCount--;
                        hasRotten = true;
                    }
                }
            }
            // 只有在本轮有橘子腐烂时，才增加时间
            if (hasRotten) {
                time++;
            }
        }

        // 如果还有新鲜橘子，返回 -1
        return freshCount == 0 ? time : -1;
    }
}
```

## 49.[课程表](https://leetcode.cn/problems/course-schedule/)(x)

你这个学期必须选修 `numCourses` 门课程，记为 `0` 到 `numCourses - 1` 。

在选修某些课程之前需要一些先修课程。 先修课程按数组 `prerequisites` 给出，其中 `prerequisites[i] = [ai, bi]` ，表示如果要学习课程 `ai` 则 **必须** 先学习课程 `bi` 。

- 例如，先修课程对 `[0, 1]` 表示：想要学习课程 `0` ，你需要先完成课程 `1` 。

请你判断是否可能完成所有课程的学习？如果可以，返回 `true` ；否则，返回 `false` 。

```java
canFinishclass Solution49{
    public boolean canFinsh(int numCourses,int [][] prerequisites){
        int[] indegress  = new int[numCourses];
        List<List<Integer>> adjacency = new ArrayList<>();
        Queue<Integer> queue = new LinkedList<>();
        for (int i =0;i<numCourses;i++){
            adjacency.add(new ArrayList<>());
        }
        for (int[] cp:prerequisites){
            indegress[cp[0]]++;
            adjacency.get(cp[1]).add(cp[0]);
        }
        for (int i =0;i<numCourses;i++){
            if (indegress[i]==0) queue.add(i);
        }
        while (!queue.isEmpty()){
            int pre  = queue.poll();
            numCourses--;
            for (int cur:adjacency.get(pre))
                if (--indegress[cur]==0) queue.add(cur);
        }
        return numCourses==0;
    }
}
```

# 回溯

## 50.[全排列](https://leetcode.cn/problems/permutations/)

给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列* 。你可以 **按任意顺序** 返回答案

```java
class Solution50{
    List<Integer> nums;
    List<List<Integer>> res;
    void swap(int a,int b){
        int tmp = nums.get(a);
        nums.set(a,nums.get(b));
        nums.set(b,tmp);
    }
    void dfs(int x){
        if (x==nums.size()-1){
            res.add(new ArrayList<>(nums));
            return;
        }
        for (int i =x;i<nums.size();i++){
            swap(i,x);
            dfs(x+1);
            swap(i,x);
        }
    }
    public List<List<Integer>> permute(int[] nums){
        this.res = new ArrayList<>();
        this.nums = new ArrayList<>();
        for (int num:nums){
            this.nums.add(num);
        }
        dfs(0);
        return res;
    }
}

```

就是找到所有可能的情况

swap用于交换，先固定，然后再**回溯**，即固定 `nums[i]` 为当前位元素。

dfs为深度搜索，搜索x的下一个之后的结果

## 51.[子集](https://leetcode.cn/problems/subsets/)

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

```java
class Solution51{
    public List<List<Integer>> subsets(int[] nums){
        List<List<Integer>> res = new ArrayList<>();
        dfs(nums,new ArrayList<>(),0,res);
        return res;
    }
    public void dfs(int[] nums,List<Integer> row,int n,List<List<Integer>> res){
        if (n ==nums.length){
            res.add(new ArrayList<>(row));
            return;
        }
        int nthNumber = nums[n];
        dfs(nums,row,n+1,res);
        row.add(nthNumber);
        dfs(nums,row,n+1,res);
        row.remove(row.size()-1);
    }
}

```

主要使用的方法就是我们的dfs

任何一个数字都有选择和不选择两种状态。所以结果集就是这个状态二叉树的所有叶子结点

第n位等于长度的时候就说明完成了，把集合封装进res

先搜索n+1然后取当前元素nth，再dfs

然后回溯，撤销选择的元素

## 52.[电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。答案可以按 **任意顺序** 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

```java
class Solution52{
    private String letterMap[] = {
            " ",    //0
            "",     //1
            "abc",  //2
            "def",  //3
            "ghi",  //4
            "jkl",  //5
            "mno",  //6
            "pqrs", //7
            "tuv",  //8
            "wxyz"  //9
    };
    private ArrayList<String> res;

    public List<String> letterCombinations(String digits){
        res = new ArrayList<String>();
        if (digits.equals("")){
            return res;
        }
        findCombination(digits,0,"");
        return res;
    }
    private void findCombination(String digits,int index,String s){
        if (index==digits.length()){
            res.add(s);
            return;
        }
        Character c = digits.charAt(index);
        String letters = letterMap[c-'0'];
        for (int i =0;i<letters.length();i++){
            findCombination(digits,index+1,s+letters.charAt(i));
            
        }
        return;
    }
}
```

问题转化成了从根节点到空节点一共有多少条路径；

直接递归进行搜索

主要是字符的形式进行递归

## 53.[组合总和](https://leetcode.cn/problems/combination-sum/)

给你一个 **无重复元素** 的整数数组 `candidates` 和一个目标整数 `target` ，找出 `candidates` 中可以使数字和为目标数 `target` 的 所有 **不同组合** ，并以列表形式返回。你可以按 **任意顺序** 返回这些组合。

`candidates` 中的 **同一个** 数字可以 **无限制重复被选取** 。如果至少一个数字的被选数量不同，则两种组合是不同的。 

对于给定的输入，保证和为 `target` 的不同组合数少于 `150` 个。

```java
class Solution53{
    void backtrack(List<Integer> state,int target,int[] choices,int start,List<List<Integer>> res){
        if (target==0){
            res.add(new ArrayList<>(state));
            return;
        }
        for (int i=start; i<choices.length;i++){
            if (target-choices[i]<0){
                break;
            }
            state.add(choices[i]);
            backtrack(state,target-choices[i],choices,i,res);
            state.remove(state.size()-1);
        }
    }
    public List<List<Integer>> combinationSum(int[] candidates,int target){
        List<Integer> state = new ArrayList<>();
        Arrays.sort(candidates);
        int start = 0;
        List<List<Integer>> res = new ArrayList<>();
        backtrack(state,target,candidates,start,res);
        return res;
    }
}
```

主要就是解析这个函数

```java
 // 子集和等于 target 时，记录解
        if (target == 0) {
            res.add(new ArrayList<>(state));
            return;
        }
        // 遍历所有选择
        // 剪枝二：从 start 开始遍历，避免生成重复子集
        for (int i = start; i < choices.length; i++) {
            // 剪枝一：若子集和超过 target ，则直接结束循环
            // 这是因为数组已排序，后边元素更大，子集和一定超过 target
            if (target - choices[i] < 0) {
                break;
            }
            // 尝试：做出选择，更新 target, start
            state.add(choices[i]);
            // 进行下一轮选择
            backtrack(state, target - choices[i], choices, i, res);
            // 回退：撤销选择，恢复到之前的状态
            state.remove(state.size() - 1);
        }

```

## 54.[括号生成](https://leetcode.cn/problems/generate-parentheses/)(x)

数字 `n` 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。

```java
class Solution54{
    private int n;
    private final List<String> ans = new ArrayList<>();
    private char[] path;

    public List<String> generateParenthesis(int n){
        this.n = n;
        path = new char[n*2];
        dfs(0,0);
        return ans;
    }
    
    
    private void dfs(int i ,int open){
        if (i==n*2){
            ans.add(new String(path));
            return;
        }
        if (open<n){
            path[i] = '(';
            dfs(i+1,open+1);
        }
        if (i-open<open){
            path[i] = ')';
            dfs(i+1,open);
        }
    }
}
```

对于括号字符串的任意前缀，**右括号的个数不能超过左括号的个数**。

代码中的 open < n 限制左括号至多填 n 个，i - open < open 限制右括号至多填 open 个（不能超过左括号的个数）。由于一共要填 2n 个括号，那么当我们递归到终点时：

如果左括号少于 n 个，那么右括号也会少于 n 个，与 i == m 矛盾，因为每填一个括号 i 都会增加 1。
如果左括号超过 n 个，与 open < n 矛盾，这句话限制了左括号至多填 n 个。
所以递归到终点时，左括号恰好填了 n 个，此时右括号也恰好填了 2n−n=n 个。

## 55.[单词搜索](https://leetcode.cn/problems/word-search/)(x)

给定一个 `m x n` 二维字符网格 `board` 和一个字符串单词 `word` 。如果 `word` 存在于网格中，返回 `true` ；否则，返回 `false` 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用

```java
class Solution55{
    static int[][] points = new int[][]{{1, 0}, {-1, 0}, {0, -1}, {0, 1}};

    public boolean exist(char[][] board,String word){
        if (board.length==0){
            return false;
        }
        char[] chars =word.toCharArray();
        for (int i =0;i<board.length;i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (dfs(i, j, board, 0, chars)) {
                    ;
                    return true;
                }
            }
        }

        return false;
    }
    private  boolean dfs(int x,int y,char[][] board,int index,char[] chars){
        if (x<0||x>board.length-1||
        y<0||y>board[0].length-1||
                board[x][y]!=chars[index]
        ){
            return false;
        }
        if (index==chars.length-1){
            return true;
        }
        board[x][y] ='\0';
        for (int i =0;i<4;i++){
            if (dfs(x+points[i][0],y+points[i][1],board,index+1,chars)){
                return true;
            }
        }
        board[x][y] =chars[index];
        return false;
    }
}
```

```java
class Solution {
    //以某点为原点的上下左右四个方向
    static int[][] points = new int[][]{{1, 0}, {-1, 0}, {0, -1}, {0, 1}};
    
    public boolean exist(char[][] board, String word) {
        //特例
        if(board.length == 0){
            return false;
        }
        //开始遍历
        char[] chars = word.toCharArray();
        for(int i = 0; i < board.length; i++){
            for(int j = 0; j < board[0].length; j++){
                //存在符合条件的直接返回
                if(dfs(i, j, board, 0, chars)){
                   return true; 
                }
            }
        }
        return false;
    }
    //开始进行递归寻找
    private boolean dfs(int x, int y, char[][] board, int index , char[] chars){
        //超出边界直接返回,board[x][y]位置和单词对应位置的字符不相等也要退出
        if(x < 0 || x > board.length - 1 || 
           y < 0 || y > board[0].length -1 ||
           board[x][y] != chars[index]){
            return false;
        }
        //当索引已经是最后一位说明找到了匹配的直接返回true；
        if(index == chars.length - 1){
            return true;
        }
        //走过的路全部置空，最上面的判定条件的判定（\0表示空字符）
        board[x][y] = '\0'; 
        //以(x,y)为原点向四周递归
        for(int i = 0; i < 4; i++){
            //当判定存在的后直接退出并返回true,能到这一步的一定是当前index所在的位置的字母
            //和board[x][y]相匹配的
            if(dfs(x + points[i][0], y + points[i][1], board, index + 1, chars)){
                return true;
            }
        }
        //回溯
        board[x][y] = chars[index];
        //都没有符合则为false
        return false;
    }
}
```

## 56.[分割回文串](https://leetcode.cn/problems/palindrome-partitioning/)

给你一个字符串 `s`，请你将 `s` 分割成一些 子串，使每个子串都是 **回文串** 。返回 `s` 所有可能的分割方案。

```java
class Solution56{
    private final List<List<String>> ans = new ArrayList<>();
    private final List<String> path = new ArrayList<>();
    private String s;

    public List<List<String>> partition(String s) {
        this.s = s;
        dfs(0);
        return ans;
    }

    private void dfs(int i){
        if (i==s.length()){//分割完毕
            ans.add(new ArrayList<>(path));
            return;
        }
        for (int j=i;j<s.length();j++){//枚举结束的位置
            if (isPalindrom(i,j)){
                path.add(s.substring(i,j+1));//分割
                dfs(j+1);
                path.remove(path.size()-1);//回溯
            }
        }
    }
    private boolean isPalindrom(int left,int right){
        while (left<right){
            if (s.charAt(left++)!=s.charAt(right--)){
                return false;
            }
        }
        return true;
    }
}
```

# 栈

## 57.[有效的括号](https://leetcode.cn/problems/valid-parentheses/)

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。
3. 每个右括号都有一个对应的相同类型的左括号。

```java
class Solution57{
    public boolean isValid(String s){
        if (s.isEmpty())
            return true;
        Stack<Character> stack = new Stack<Character>();
        for (char c:s.toCharArray()){
            if (c=='(')
                stack.push(')');
            else if (c=='{')
                stack.push('}');
            else if (c=='[')
                stack.push(']');
            else if (stack.empty()||c!=stack.pop())
                return false;
        }
       return stack.empty();
    }
}
```

这个括号要求成双成对，有左必有右

若遇到左括号入栈，遇到右括号时将对应栈顶左括号出栈，则遍历完所有括号后 `stack` 仍然为空；

遍历完stack为空即为合理



## 58.[最小栈](https://leetcode.cn/problems/min-stack/)

设计一个支持 `push` ，`pop` ，`top` 操作，并能在常数时间内检索到最小元素的栈。

实现 `MinStack` 类:

- `MinStack()` 初始化堆栈对象。
- `void push(int val)` 将元素val推入堆栈。
- `void pop()` 删除堆栈顶部的元素。
- `int top()` 获取堆栈顶部的元素。
- `int getMin()` 获取堆栈中的最小元素。

push() 方法： 每当push()新值进来时，如果 小于等于 min_stack 栈顶值，则一起 push() 到 min_stack，即更新了栈顶最小值；
pop() 方法： 判断将 pop() 出去的元素值是否是 min_stack 栈顶元素值（即最小值），如果是则将 min_stack 栈顶元素一起 pop()，这样可以保证 min_stack 栈顶元素始终是 stack 中的最小值。

```java
class MinStack{
    private Stack<Integer> stack;
    private Stack<Integer> min_stack;
    public MinStack(){
        stack = new Stack<>();
        min_stack = new Stack<>();
    }
    public void push(int x){
        stack.push(x);
        if (min_stack.isEmpty()||x<=min_stack.peek())
            min_stack.push(x);
    }
    public void pop(){
        if (stack.pop().equals(min_stack.peek()))
            min_stack.pop();
    }
    public int top(){
        return stack.peek();
    }
    public int getMin(){
        return min_stack.peek();
    }
}
```

## 59.[字符串解码](https://leetcode.cn/problems/decode-string/)

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: `k[encoded_string]`，表示其中方括号内部的 `encoded_string` 正好重复 `k` 次。注意 `k` 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 `k` ，例如不会出现像 `3a` 或 `2[4]` 的输入。

```java
class Solution59{
    public String decodeString(String s){
        Stack<Integer> countstack = new Stack<>();
        Stack<StringBuilder> stringstack = new Stack<>();
        StringBuilder currentString = new StringBuilder();
        int k = 0;//重复次数


        for (char c:s.toCharArray()){
            if (Character.isDigit(c)){//如果c为数字的话
                k = k*10+(c-'0');//多位数字
            }else if (c=='['){//开始
                countstack.push(k);
                k=0;
                stringstack.push(currentString);
                currentString = new StringBuilder();
            }else if (c==']'){//结束
                int repeat = countstack.pop();
                StringBuilder sb = stringstack.pop();
                for (int i =0;i<repeat;i++){
                    sb.append(currentString);//
                }//开始重复
                currentString = sb;//更新拼接之后的字符串
            }else {
                currentString.append(c);
            }
        }
        return currentString.toString();
    }
}
```

## 60.[每日温度](https://leetcode.cn/problems/daily-temperatures/)

给定一个整数数组 `temperatures` ，表示每天的温度，返回一个数组 `answer` ，其中 `answer[i]` 是指对于第 `i` 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。

```java
class Solution60{
    public int[] dailyTemperatures1(int[] T) {
        int length = T.length;
        int[] result = new int[length];

        for (int i = length - 2; i >= 0; i--) {
            for (int j = i + 1; j < length; j += result[j]) {
                if (T[j] > T[i]) {
                    result[i] = j - i;
                    break;
                } else if (result[j] == 0) {
                    result[i] = 0;
                    break;

                }
            }
        }
        return result;
    }
    public int[] dailyTemperatures(int[] temperatures){
        int n  = temperatures.length;
        int[] ans = new int[n];

        Deque<Integer> st  = new ArrayDeque<>();

        for (int i =n-1;i>=0;i--){
            int t = temperatures[i];
            while (!st.isEmpty()&&t>=temperatures[st.peek()]){
                st.pop();
            }
            if (!st.isEmpty()){
                ans[i] = st.peek()-i;
            }
            st.push(i);
        }
        return ans;

    }
```

从后面开始遍历

t存temperatures的第i个数据

st不为空的时候，ans[i]个数据是st的顶端的第i个作为候选就是那个答案，第i天

然后压入第I个

之后再不为空，，并且右更大的时候，出栈，然后新的最高温度就进栈可以继续比较了

最后返回ans结果

# 堆

## 61.[数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/)（topk）

给定整数数组 `nums` 和整数 `k`，请返回数组中第 `**k**` 个最大的元素。

请注意，你需要找的是数组排序后的第 `k` 个最大的元素，而不是第 `k` 个不同的元素。

你必须设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

```
class Solution61{
    public int findKthLargest(int[] nums,int k){
        Arrays.sort(nums);
        return nums[nums.length-k];
    }
}
```

直接排序，返回n-k个元素即可

## 62.[前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)（topk）(代码优美)

给你一个整数数组 `nums` 和一个整数 `k` ，请你返回其中出现频率前 `k` 高的元素。你可以按 **任意顺序** 返回答案。

```java
class Solution{
    public int[] topKFrequent(int[] nums,int k){
        // 统计每个数字出现的次数
        Map<Integer,Integer> counter = IntStream.of(nums).boxed().collect(Collectors.toMap(e->e,e->1,Integer::sum));
        // 定义小根堆，根据数字频率自小到大排序
        Queue<Integer> pq = new PriorityQueue<>((v1,v2)->counter.get(v1)-counter.get(v2));
        counter.forEach((num,cnt)->{
            if (pq.size()<k){
                pq.offer(num);
            }else if (counter.get(pq.peek())<cnt){
                pq.poll();
                pq.offer(num);
            }
        });
        int[] res = new int[k];
        int idx = 0;
        for (int num:pq){
            res[idx++] = num;
        }
        return res;
    }

}

```

# 贪心算法

## 63.[买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。

你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0` 。

```java
class Solution63{
    public int maxProfit(int[] prices){
        int cost = Integer.MAX_VALUE,profit = 0;
        for (int price:prices){
            cost = Math.min(cost,price);
            profit = Math.max(profit,price-cost);
        }
        return profit;
    }
}
```

简单的问题，直接使用Math函数即可。然后遍历一下

## 64.[跳跃游戏](https://leetcode.cn/problems/jump-game/)

给你一个非负整数数组 `nums` ，你最初位于数组的 **第一个下标** 。数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标，如果可以，返回 `true` ；否则，返回 `false` 。

从左到右遍历 nums，同时维护能跳到的最远位置 mx，初始值为 0。
如果 i>mx，说明无法跳到 i，返回 false。
否则，用 i+nums[i]更新 mx 的最大值。
如果循环中没有返回 false，那么最后返回 true。

```java
class Solution64{
    public boolean canJump(int[] nums){
        int mx = 0;
        for (int i=0;mx<nums.length-1;i++){
            if (i>mx) return false;
            mx = Math.max(mx,i+nums[i]);
        }
        return true;
    }
}


```

## 65.[跳跃游戏 II](https://leetcode.cn/problems/jump-game-ii/)

给定一个长度为 `n` 的 **0 索引**整数数组 `nums`。初始位置为 `nums[0]`。

每个元素 `nums[i]` 表示从索引 `i` 向后跳转的最大长度。换句话说，如果你在 `nums[i]` 处，你可以跳转到任意 `nums[i + j]` 处:

- `0 <= j <= nums[i]` 
- `i + j < n`

返回到达 `nums[n - 1]` 的最小跳跃次数。生成的测试用例可以到达 `nums[n - 1]`。

```java
class Solution65{
    public int jump(int[] nums){
        int end = 0;
        int maxPosition = 0;
        int steps = 0;
        for (int i =0;i>nums.length-1;i++){
            maxPosition = Math.max(maxPosition,nums[i]+i);
            if (i==end){
                end = maxPosition;
                steps++;
            }
        }
        return steps;
    }
}

```

跟上面64题差不多，只不过遇到了边界的时候，边界更新为最大值，然后步数++

最后返回统计的步数

## 66.[划分字母区间](https://leetcode.cn/problems/partition-labels/)

给你一个字符串 `s` 。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。例如，字符串 `"ababcc"` 能够被分为 `["abab", "cc"]`，但类似 `["aba", "bcc"]` 或 `["ab", "ab", "cc"]` 的划分是非法的。

注意，划分结果需要满足：将所有划分结果按顺序连接，得到的字符串仍然是 `s` 。

返回一个表示每个字符串片段的长度的列表。

遍历 s，计算字母 c 在 s 中的最后出现的下标 last[c]。
初始化当前正在合并的区间左右端点 start=0, end=0。
再次遍历 s，由于当前区间必须包含所有 s[i]，所以用 last[s[i]] 更新区间右端点 end 的最大值。
如果发现 end=i，那么当前区间合并完毕，把区间长度 end−start+1 加入答案。然后更新 start=i+1 作为下一个区间的左端点。
遍历完毕，返回答案。

```java
class Solution66{
    public List<Integer> partitionLabels(String S){
        char[] s = S.toCharArray();
        int n = s.length;
        int [] last = new int[26];
        for (int i =0;i<n;i++){
            last[s[i]-'a']  = i;
        }
        List<Integer> ans = new ArrayList<>();
         int start = 0,end =0;
        for (int i =0;i<n;i++){
            end = Math.max(end,last[s[i]-'a']);
            if (end == i){
                ans.add(end-start+1);
                start = i+1;
            }
        }
        return ans;
    }
}

```

# 技巧

## 67.[只出现一次的数字](https://leetcode.cn/problems/single-number/)

给你一个 **非空** 整数数组 `nums` ，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

你必须设计并实现线性时间复杂度的算法来解决此问题，且该算法只使用常量额外空间。

```java
class Solution67{
    public int  singleNumber(int[] nums){
        int x=0;
        for (int num:nums){
            x^=num;
        }
        return x;
    }
}

```

这样就能找出只出现一次的数字了，因为只出现一次肯定是没有方的

## 68.[多数元素](https://leetcode.cn/problems/majority-element/)

给定一个大小为 `n` 的数组 `nums` ，返回其中的多数元素。多数元素是指在数组中出现次数 **大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。



初始化： 票数统计 votes = 0 ， 众数 x。
循环： 遍历数组 nums 中的每个数字 num 。
当 票数 votes 等于 0 ，则假设当前数字 num 是众数。
当 num = x 时，票数 votes 自增 1 ；当 num != x 时，票数 votes 自减 1 。
返回值： 返回 x 即可。

```java
class Solution68{
    public int majorityElement(int[] nums){
        int x=0,votes = 0;
        for (int num:nums){
            if (votes==0) x=num;
            votes += (num ==x?1:-1);
        }
        return x;
    }
}

```

## 69.[颜色分类](https://leetcode.cn/problems/sort-colors/)

给定一个包含红色、白色和蓝色、共 `n` 个元素的数组 `nums` ，**[原地](https://baike.baidu.com/item/原地算法)** 对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

我们使用整数 `0`、 `1` 和 `2` 分别表示红色、白色和蓝色。



必须在不使用库内置的 sort 函数的情况下解决这个问题。

```java
class Solution69{
    public void sortColors(int[] nums) {
        int len = nums.length;
        if (len<2){
            return;
        }
        int zero  = -1;
        int two = len-1;
        int i =0;
        while (i<two){
            if (nums[i]==0){
                zero++;
                swap(nums,i,zero);
                i++;
            }else if (nums[i]==1){
                i++;
            }else {
                swap(nums,i,two);
                two--;
            }
        }

    }
    private void swap(int[] nums,int index1,int index2){
        int temp = nums[index1];
        nums[index1] = nums[index2];
        nums[index2] = temp;
    }
}

```

就是一个字，遇到0往最前面放，遇到2往最后面放。

然后交换就可以了

## 70.[下一个排列](https://leetcode.cn/problems/next-permutation/)

整数数组的一个 **排列** 就是将其所有成员以序列或线性顺序排列。

- 例如，`arr = [1,2,3]` ，以下这些都可以视作 `arr` 的排列：`[1,2,3]`、`[1,3,2]`、`[3,1,2]`、`[2,3,1]` 。

整数数组的 **下一个排列** 是指其整数的下一个字典序更大的排列。更正式地，如果数组的所有排列根据其字典顺序从小到大排列在一个容器中，那么数组的 **下一个排列** 就是在这个有序容器中排在它后面的那个排列。如果不存在下一个更大的排列，那么这个数组必须重排为字典序最小的排列（即，其元素按升序排列）。

- 例如，`arr = [1,2,3]` 的下一个排列是 `[1,3,2]` 。
- 类似地，`arr = [2,3,1]` 的下一个排列是 `[3,1,2]` 。
- 而 `arr = [3,2,1]` 的下一个排列是 `[1,2,3]` ，因为 `[3,2,1]` 不存在一个字典序更大的排列。

给你一个整数数组 `nums` ，找出 `nums` 的下一个排列。

必须**[ 原地 ](https://baike.baidu.com/item/原地算法)**修改，只允许使用额外常数空间。

```java
class Solution
{
    private void swap(int[] nums,int i,int j){
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
    private void reverse(int[] nums,int left,int right){
        while (left<right){
            swap(nums,left++,right--);
        }
    }
    public void nextPermutation(int[] nums) {
        int n = nums.length;
        int i = n-2;
        while (i>=0&&nums[i]>=nums[i+1]){
            i--;
        }
        if (i>=0){
            int j = n-1;
            while (nums[j]<=nums[i]){
                j--;
            }
            swap(nums,i,j);
        }
        reverse(nums,i+1,n-1);
    }
}
```

1.从右向左，找第一个小于右侧相邻数字的数 *x*

2.找 *x* 右边最小的大于 *x* 的数 *y*，交换 *x* 和 *y*

3.反转 *y* 右边的数，把右边的数变成最小的排列

## 71.[寻找重复数](https://leetcode.cn/problems/find-the-duplicate-number/)

给定一个包含 `n + 1` 个整数的数组 `nums` ，其数字都在 `[1, n]` 范围内（包括 `1` 和 `n`），可知至少存在一个重复的整数。

假设 `nums` 只有 **一个重复的整数** ，返回 **这个重复的数** 。

你设计的解决方案必须 **不修改** 数组 `nums` 且只用常量级 `O(1)` 的额外空间。

```java
class Solution{
    public int findDuplicate(int[] nums){
        int s = 0;
        int f = 0;
        while (true){
            f = nums[f];
            f = nums[f];
            s = nums[s];
            if (s == f) break;
        }
        int ptr = 0;
        while (ptr!=s){
            ptr = nums[ptr];
            s = nums[s];
        }
        return ptr;
    }
}
```

$$
ptr == slow 时说明检测到重复元素，两个重复元素同时指向环的入口。
$$

# 动态规划

## 72.[爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

```java
public class Solution {
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

## 73.[杨辉三角](https://leetcode.cn/problems/pascals-triangle/)

给定一个非负整数 *`numRows`，*生成「杨辉三角」的前 *`numRows`* 行。

在「杨辉三角」中，每个数是它左上方和右上方的数的和。

```java
class Solution73{
    public List<List<Integer>> generate(int numRows){
        List<List<Integer>> c = new ArrayList<>(numRows);
        if (numRows <= 0) return c;
        c.add(List.of(1));
        for (int i =1;i<numRows;i++){//每一行的实现
            List<Integer> row = new ArrayList<>(i+1);
            row.add(1);//第一个元素
            for (int j =1;j<i;j++){//相加的实现
                row.add(c.get(i-1).get(j-1)+c.get(i-1).get(j));
            }
            row.add(1);//最后一个元素
            c.add(row);
        }
        return c;
    }
}


```

## 74.[打家劫舍](https://leetcode.cn/problems/house-robber/)

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。

给定一个代表每个房屋存放金额的非负整数数组，计算你 **不触动警报装置的情况下** ，一夜之内能够偷窃到的最高金额。

```java
class Solution74 {
    public int rob(int[] nums) {
        if (nums.length == 0) {
            return 0;
        }
        int N = nums.length;
        int[] dp = new int[N + 1]; // 创建 DP 数组，dp[i] 代表抢劫前 i 间房子时的最大金额
        
        dp[0] = 0;         // 不抢任何房子
        dp[1] = nums[0];   // 只有一间房子时，抢它

        for (int k = 2; k <= N; k++) {
            dp[k] = Math.max(dp[k - 1], nums[k - 1] + dp[k - 2]);
            // 选择：不抢当前房子(dp[k-1])，或抢当前房子(nums[k-1] + dp[k-2])
        }
        
        return dp[N]; // 最后一个状态就是最大金额
    }
}

```

## 75.[完全平方数](https://leetcode.cn/problems/perfect-squares/)

给你一个整数 `n` ，返回 *和为 `n` 的完全平方数的最少数量* 。

**完全平方数** 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，`1`、`4`、`9` 和 `16` 都是完全平方数，而 `3` 和 `11` 不是。

```java
import java.util.Arrays;

class Solution75 {
    public int numSquares(int n) {
        int[] f = new int[n + 1];
        Arrays.fill(f, Integer.MAX_VALUE);
        f[0] = 0;  // base case

        for (int i = 1; i * i <= n; i++) {  // 遍历所有完全平方数 i*i
            for (int j = i * i; j <= n; j++) { // 遍历目标值 j
                if (f[j - i * i] != Integer.MAX_VALUE) {  // 避免溢出
                    f[j] = Math.min(f[j], f[j - i * i] + 1);
                }
            }
        }
        return f[n];
    }
}

```

## 76.[零钱兑换](https://leetcode.cn/problems/coin-change/)

给你一个整数数组 `coins` ，表示不同面额的硬币；以及一个整数 `amount` ，表示总金额。

计算并返回可以凑成总金额所需的 **最少的硬币个数** 。如果没有任何一种硬币组合能组成总金额，返回 `-1` 。

你可以认为每种硬币的数量是无限的。

```java
class Solution76{
    public int coinChange(int[] coins,int amount){
        int[] f = new int[amount+1];
        Arrays.fill(f,Integer.MAX_VALUE/2);
        f[0]=0;
        for (int x:coins){
            for (int c = x;c<=amount;c++){
                f[c] = Math.min(f[c],f[c-x]+1);
            }
        }
        int ans = f[amount];
        return ans<Integer.MAX_VALUE/2?ans:-1;
    }
}
```

跟上面的是一个思路，只不过不是平方罢了

然后加了个判断

如果没有任何一种硬币组合能组成总金额，返回 `-1` 。

## 77.[单词拆分](https://leetcode.cn/problems/word-break/)

给你一个字符串 `s` 和一个字符串列表 `wordDict` 作为字典。如果可以利用字典中出现的一个或多个单词拼接出 `s` 则返回 `true`。

**注意：**不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。

```java
import java.util.*;

class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        // 计算字典中最长的单词长度
        int maxlen = 0;
        for (String word : wordDict) {
            maxlen = Math.max(maxlen, word.length());
        }//maxlen获得

        // 将 wordDict 存入 HashSet 以提高查询效率
        Set<String> words = new HashSet<>(wordDict);
        
        int n = s.length();
        boolean[] f = new boolean[n + 1];
        f[0] = true; // 空字符串可以被拆分

        // 遍历字符串 s 的每个前缀
        for (int i = 1; i <= n; i++) {
            for (int j = Math.max(i - maxlen, 0); j < i; j++) {  // 修正 j 的范围
                if (f[j] && words.contains(s.substring(j, i))) {//从j到i在words之中
                    f[i] = true;
                    break;
                }
            }
        }
        
        return f[n]; // 返回能否拆分整个字符串
    }
}

```

## 78.[最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/)

给你一个整数数组 `nums` ，找到其中最长严格递增子序列的长度。

**子序列** 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，`[3,6,2,7]` 是数组 `[0,3,1,6,2,2,7]` 的子序列。

动态规划，

```java
class Solution78{
    public int lengthOfLIS(int[] nums){
        if (nums.length==0) return 0;
        int[] dp = new int[nums.length];
        int res = 0;
        Arrays.fill(dp,1);
        for (int i =0;i<nums.length;i++){
            for (int j=0;j<i;j++){
                if (nums[j]<nums[i]) dp[i] = Math.min(dp[i],dp[j]+1);
            }
            res = Math.max(res,dp[i]);
        }
        return res;
    }
}

```

递归开始，当nums[j]<nums[i]个的时候，j是更小的，这个时候最长的子序列就能变的更长

也就是加1 dp[i]长度就+1 然后递归完res就是最大的dp[i]的长度

最后完成res的结果

最后返回res结果

## 79.[乘积最大子数组](https://leetcode.cn/problems/maximum-product-subarray/)

给你一个整数数组 `nums` ，请你找出数组中乘积最大的非空连续 子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

测试用例的答案是一个 **32-位** 整数。

因为是负负得正，所以可能是两个负数相乘是最大的

所以不仅要维护一个最大值，还要维护一个最小值

```java
class Solution79{
    public int maxProduct(int[] nums){
        int max = Integer.MIN_VALUE,imax = 1,imin=1;
        for (int i =0;i<nums.length;i++){
            if (nums[i]<0){
                int tmp = imax;
                imax = imin;
                imin= tmp;
            }
            imax = Math.max(imax*nums[i],nums[i]);
            imin = Math.min(imin*nums[i],nums[i]);
            max = Math.max(max,imax);
        }
        return max;
    }

}


```

## 80.[不同路径](https://leetcode.cn/problems/unique-paths/)

一个机器人位于一个 `m x n` 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

问总共有多少条不同的路径？

dp[i】[j】 = dp[i-1】[j] + dp[i][j-1】

对于第一行 `dp[0][j]`，或者第一列 `dp[i][0]`，由于都是在边界，所以只能为 `1`

```java
class Solution80{
    public int uniquePaths(int m,int n){
        int[][] dp = new int[m][n];
        for (int i =0;i<n;i++) dp[0][i] =1;
        for (int i=0;i<m;i++) dp[i][0] = 1;
        for (int i =1;i<m;i++){
            for (int j=1;j<n;j++){
                dp[i][j] = dp[i-1][j]+dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
}
```

## 81.[ 最小路径和](https://leetcode.cn/problems/minimum-path-sum/)

给定一个包含非负整数的 `*m* x *n*` 网格 `grid` ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：**每次只能向下或者向右移动一步。



```java
class Solution81{
    public int minPathSum(int[][] grid){
        for(int i =0;i<grid.length;i++){
            for (int j =0;j<grid[0].length;j++){
                if (i==0&&j==0) continue;
                else if (i==0) grid[i][j] = grid[i][j-1]+grid[i][j];
                else if (j==0) grid[i][j] = grid[i-1][j]+grid[i][j];
                else grid[i][j] += Math.min(grid[i - 1][j], grid[i][j - 1]);
            }
        }
        return grid[grid.length-1][grid[0].length-1];
    }
}
```

跟上面的一样

遇到边界下一个

然后遇到真正的就找最小的路走。然后最右下角的那个就是最小的路径和

## 82.[最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)(x)

给你一个字符串 `s`，找到 `s` 中最长的 回文 子串。

使用：Manacher 算法

```java
class Solution82 {
    public String longestPalindrome(String s) {
        int n = s.length();
        if (n == 0) return "";

        // 预处理字符串
        char[] t = new char[n * 2 + 3];
        Arrays.fill(t, '#');
        t[0] = '^';
        t[n * 2 + 2] = '$';
        for (int i = 0; i < n; i++) {
            t[i * 2 + 2] = s.charAt(i);
        }

        int[] halfLen = new int[t.length]; // 这里大小应该与 t 一致
        int maxI = 0;
        int boxM = 0, boxR = 0;

        // Manacher's Algorithm
        for (int i = 1; i < t.length - 1; i++) {
            // 计算当前点的初始回文半径
            int hl = (i < boxR) ? Math.min(halfLen[2 * boxM - i], boxR - i) : 1;

            // 尝试扩展回文半径
            while (t[i - hl] == t[i + hl]) {
                hl++;
            }

            halfLen[i] = hl;

            // 更新右边界
            if (i + hl > boxR) {
                boxM = i;
                boxR = i + hl;
            }

            // 更新最长回文中心
            if (halfLen[i] > halfLen[maxI]) {
                maxI = i;
            }
        }

        // 计算原字符串中的起始索引
        int start = (maxI - halfLen[maxI]) / 2;
        int end = start + halfLen[maxI] - 1;

        return s.substring(start, end);
    }
}
```

```
Manacher 模板
        # 将 s 改造为 t，这样就不需要讨论 len(s) 的奇偶性，因为新串 t 的每个回文子串都是奇回文串（都有回文中心）
        # s 和 t 的下标转换关系：
        # (si+1)*2 = ti
        # ti/2-1 = si
        # ti 为偶数，对应奇回文串（从 2 开始）
        # ti 为奇数，对应偶回文串（从 3 开始）
```

暴力破解法：

等待补充。。。。 2025.4.11

## 83.[ 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/)

给定两个字符串 `text1` 和 `text2`，返回这两个字符串的最长 **公共子序列** 的长度。如果不存在 **公共子序列** ，返回 `0` 。

一个字符串的 **子序列** 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。

- 例如，`"ace"` 是 `"abcde"` 的子序列，但 `"aec"` 不是 `"abcde"` 的子序列。

两个字符串的 **公共子序列** 是这两个字符串所共同拥有的子序列。

```java
class Solution83{
    public int longestCommonSubsequence(String text1,String text2){
        char[] s = text1.toCharArray();
        char[] t = text2.toCharArray();
        int n = s.length;
        int m = t.length;
        int[][] f = new int[n+1][m+1];//初
        for (int i =0;i<n;i++){
            for (int j =0;j<m;j++){
                f[i+1][j+1] = s[i]==t[j]?f[i][j]+1:
                        Math.max(f[i][j+1],f[i+1][j]);
            }
        }
        return f[n][m];
    }
}



```

其中最主要的就是这一句

```
f[i+1][j+1] = s[i]==t[j]?f[i][j]+1:
                        Math.max(f[i][j+1],f[i+1][j]);
```

下一项赋值为：

- 如果含有公共的部分，也就是当前相等的话，**f[i][j }+1 长度+1

- 不含有的话，去下或者右的大的去寻找

- 最后这个值在
  $$
  f[n][m]
  $$
  

这个右下角上

## 84.[编辑距离](https://leetcode.cn/problems/edit-distance/)

给你两个单词 `word1` 和 `word2`， *请返回将 `word1` 转换成 `word2` 所使用的最少操作数* 。

你可以对一个单词进行如下三种操作：

- 插入一个字符
- 删除一个字符
- 替换一个字

```java
class Solution84{
    public int minDistance(String text1,String text2){
        char[] t = text2.toCharArray();
        int m = t.length;
        int[] f = new int[m+1];
        for (int j=1;j<=m;j++){
            f[j] = j;
        }
        for (char x:text1.toCharArray()){
            int pre = f[0];
            f[0]++;
            for (int j =0;j<m;j++){
                int tmp = f[j+1];
                f[j+1] = x==t[j]?pre:Math.min(Math.min(f[j + 1], f[j]), pre) + 1;
                pre = tmp;
            }
        }
        return f[m];
    }

}
```

前面现遍历把text2遍历进数组

然后遍历text1

f[j+1]=

x==t[j]是不是想等

?pre 想等就是 tmp 也就是f[j+1]不动

:Math.min(Math.min(f[j + 1], f[j]), pre) + 1;不想打，操作+1
pre = tmp;更新前一项，也就是继续往下迭代
