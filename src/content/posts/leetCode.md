---
title: leetcode
published: 2026-01-01
description: leeetcode
image: ./cover.jpg
tags: [leetcode]
category: leetcode
draft: false
author: "oiaer"
---
# leetCode

## 哈希表

### 1.两数之和

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

```html
示例 1：
	输入：nums = [2,7,11,15], target = 9
	输出：[0,1]
	解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 
示例 2：
	输入：nums = [3,2,4], target = 6
	输出：[1,2]
示例 3：
	输入：nums = [3,3], target = 6
	输出：[0,1]
```

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer,Integer> hash = new HashMap<>();
        for(int i=0;i<nums.length;i++){
            Integer j = hash.get(target - nums[i]);
            if(j != null){
                return new int[]{j,i};
            }
            hash.put(nums[i],i);
        }
        return new int[]{};
    }
}
```

拿目标数 - 当前数 = 要找的数



## 动态规划

把大问题拆成若干重叠小问题，存下小问题答案避免重复计算，用小结果推大结果

### 1.爬楼梯

假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

```htm
示例 1：
	输入：n = 2
	输出：2
	解释：有两种方法可以爬到楼顶。
	1. 1 阶 + 1 阶
	2. 2 阶
示例 2：
	输入：n = 3
	输出：3
	解释：有三种方法可以爬到楼顶。
	1. 1 阶 + 1 阶 + 1 阶
	2. 1 阶 + 2 阶
	3. 2 阶 + 1 阶
```

```java
class Solution {
    public int climbStairs(int n) {
        if(n == 1) return 1;
        if(n == 2) return 2;
        int [] dp = new int[n+1];
        dp[1] = 1;
        dp[2] = 2;
        for(int i=3;i<=n;i++){
            //第 i 阶的走法 = 前 1 阶的走法 + 前 2 阶的走法
           dp[i]=dp[i-1]+dp[i-2];
        }
        return dp[n];
    }
}
```



### 2.杨辉三角

给定一个非负整数 *`numRows`，*生成「杨辉三角」的前 *`numRows`* 行。

```html
示例 1:
	输入: numRows = 5
	输出: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
示例 2:
	输入: numRows = 1
	输出: [[1]]
```

```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> res = new ArrayList<>();
        for(int i=0;i<numRows;i++){
             List<Integer> row = new ArrayList<>();
             for(int j=0;j<=i;j++){
                if(j==i || j==0){
                    row.add(1);
                }else{
                    //当前数 = 上一行当前位置的前一个数 + 上一行当前位置的数
                    int val = res.get(i-1).get(j-1) + res.get(i-1).get(j);
                    row.add(val);  
                }
             }
             res.add(row);
        }
        return res;
    }
}
```

#### 2.1杨辉三角

给定一个非负索引 `rowIndex`，返回「杨辉三角」的第 `rowIndex` 行。

```htmnl
示例 1:
	输入: rowIndex = 3
	输出: [1,3,3,1]
示例 2:
	输入: rowIndex = 0
	输出: [1]
示例 3:
	输入: rowIndex = 1
	输出: [1,1]
```

```java
class Solution {
    public List<Integer> getRow(int rowIndex) {
        List<Integer> list= new ArrayList<>();
        for(int i=0;i<=rowIndex;i++){
            list.add(1);
            for(int j = i-1;j>0;j--){
                //从后往前更新
                //当前位置的数 = 当前数+左边数
               list.set(j, list.get(j) + list.get(j-1));
            }
        }
        return list;
    }
}
```



## 排序

### 1.有效的字母异位词

给定两个字符串 `s` 和 `t` ，编写一个函数来判断 `t` 是否是 `s` 的 字母异位词。

（字母异位词是通过重新排列不同单词或短语的字母而形成的单词或短语，并使用所有原字母一次。）

```html
示例 1:
	输入: s = "anagram", t = "nagaram"
	输出: true
示例 2:
	输入: s = "rat", t = "car"
	输出: false
```

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        HashMap<Character, Integer> map = new HashMap<>();
        for (char c : s.toCharArray()) {
            map.put(c, map.getOrDefault(c, 0) + 1);
        }
        for (char c : t.toCharArray()) {
            // 哈希里没有这个字符 不匹配
            if (!map.containsKey(c)) {
                return false;
            }
            // 计数减 1
            map.put(c, map.get(c) - 1);
            // 减到负数，说明 t 里这个字符更多不匹配
            if (map.get(c) < 0) {
                return false;
            }
        }
        return true;
    }
}
```

### 2. 找不同

给定两个字符串 s 和 t ，它们只包含小写字母。
字符串 t 由字符串 s 随机重排，然后在随机位置添加一个字母。
请找出在 t 中被添加的字母。

```html
示例 1：
	输入：s = "abcd", t = "abcde"
	输出："e"
	解释：'e' 是那个被添加的字母。
示例 2：
	输入：s = "", t = "y"
	输出："y"
```

```java
class Solution {
    public char findTheDifference(String s, String t) {
       HashMap<Character, Integer> map = new HashMap<>();
       for (char c : s.toCharArray()) {
            map.put(c, map.getOrDefault(c, 0) + 1);
        }
        for (char c : t.toCharArray()) {
            // 如果这个字符不存在，或者次数已经是 0，就是它
            if (!map.containsKey(c) || map.get(c) == 0) {
                return c;
            }
            map.put(c, map.get(c) - 1);
        }
        
        return ' ';
    }
}
```



### 3. 第三大的数

给你一个非空数组，返回此数组中 **第三大的数** 。如果不存在，则返回数组中最大的数。

```html
示例 1：
	输入：[3, 2, 1]
	输出：1
	解释：第三大的数是 1 。

示例 2：
	输入：[1, 2]
	输出：2
	解释：第三大的数不存在, 所以返回最大的数 2 。

示例 3：
	输入：[2, 2, 3, 1]
	输出：1
	解释：注意，要求返回第三大的数，是指在所有不同数字中排第三大的数。
	此例中存在两个值为 2 的数，它们都排第二。在所有不同数字中排第三大的数为 1 。
```

```java
class Solution {
    public int thirdMax(int[] nums) {
        long first = Long.MIN_VALUE;
        long second = Long.MIN_VALUE;
        long third = Long.MIN_VALUE;

        for(int num:nums){
            if (num == first || num == second || num == third) {
                continue;
            }
            // 比第一还大 → 全员往后挤
            if (num > first) {
                third = second;
                second = first;
                first = num;
            } 
            // 比第一小，比第二大 → 后面两个挤
            else if (num > second) {
                third = second;
                second = num;
            } 
            // 比第二小，比第三大 → 只换第三
            else if (num > third) {
                third = num;
            }
        }
        return third == Long.MIN_VALUE ? (int)first : (int)third;
    }
}
```



### 4.分发饼干

```java
假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。
示例 1:
	输入: g = [1,2,3], s = [1,1]
	输出: 1
	解释: 
	你有三个孩子和两块小饼干，3 个孩子的胃口值分别是：1,2,3。
	虽然你有两块小饼干，由于他们的尺寸都是 1，你只能让胃口值是 1 的孩子满足。
	所以你应该输出 1。
示例 2:
	输入: g = [1,2], s = [1,2,3]
	输出: 2
	解释: 
	你有两个孩子和三块小饼干，2 个孩子的胃口值分别是 1,2。
	你拥有的饼干数量和尺寸都足以让所有孩子满足。
	所以你应该输出 2。
```

```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int child = 0;
        int cookie = 0;
        int res = 0;
        while(child<g.length && cookie<s.length){
            if(s[cookie] >= g[child]){
                res++;
                child++;
                cookie++;
            }else{
                cookie++;
            }
        }
        return res;
    }
}
```



### 5. 132模式

```html
给你一个整数数组 nums ，数组中共有 n 个整数。132 模式的子序列 由三个整数 nums[i]、nums[j] 和 nums[k] 组成，并同时满足：i < j < k 和 nums[i] < nums[k] < nums[j] 。
示例 1：
	输入：nums = [1,2,3,4]
	输出：false
	解释：序列中不存在 132 模式的子序列。
示例 2：
	输入：nums = [3,1,4,2]
	输出：true
	解释：序列中有 1 个 132 模式的子序列： [1, 4, 2] 。
示例 3：
	输入：nums = [-1,3,2,0]
	输出：true
	解释：序列中有 3 个 132 模式的的子序列：[-1, 3, 2]、[-1, 3, 0] 和 [-1, 2, 0] 。
```

```java
class Solution {
    public boolean find132pattern(int[] nums) {
        Stack<Integer> stack = new Stack<>();
        int second = Integer.MIN_VALUE;
        for (int i = nums.length - 1; i >= 0; i--) {
            int cur = nums[i];
            if (cur < second) {
                return true;
            }
            // 弹出所有比cur小的，更新second为最大的nums[k]
            while (!stack.isEmpty() && stack.peek() < cur) {
                second = stack.pop();
            }
            stack.push(cur);
        }
        return false;
    }
}
```

最优思路：单调递减栈（O (n)）

1. 从后往前遍历数组，栈保存候选的 `nums[k]`（2 的候选，栈保持单调递减）；
2. 用变量 `second` 记录栈中弹出的最大元素，也就是当前找到的最大合法 `nums[k]`；
3. 遍历到当前元素 num（当作 nums[j]）：
   - 只要栈不为空、栈顶 ≤ 当前 num，不断弹出栈顶并更新 `second`；
   - 此时 `second` 一定小于当前 num（`j`），满足 `nums[k]<nums[j]`；
   - 再看如果当前 num 前面存在数字 `< second`，就找到 132 模式；
   - 把当前 num 入栈，作为后续的 k 候选；
4. 遍历过程中只要出现 `num < second`，直接返回 true；遍历完无则 false。

核心逻辑

- 栈存右侧更小值，`second` 锁定最优的 2；
- 只要左侧出现比 `second` 小的数，就凑齐 i<k<j 满足 `a[i]<a[k]<a[j]`。
