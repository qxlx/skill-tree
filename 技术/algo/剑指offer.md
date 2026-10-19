[TOC]

> 笔记:qxlx  邮箱 qxlxiii@gmail.com
>

# 剑指offer

## 汇总

```
【数组】
3.数组中重复的数字  
	1.hashSet  2.数组下标法(鸽巢机制)
4.二维数组中的查找  
	1.暴力 
	2.线性查找  从右上角查找 小于列-- 大于行++
5.替换空格
    1.stringbuffer
21. 调整数组顺序使奇数位于偶数前面  
    1.双指针 2.先分开 在合并

29.顺时针打印矩阵
	左到右 上到下 右到左   下到上   
39．数组中出现次数超过一半的数字
　　　投票法
62.圆圈中最后剩下的数字
	ans = (ans+m)%i 
	
【二分查找】
11.旋转数组的最小数字【高频】
    1.二分
53 - I. 在排序数组中查找数字 I
	１.二分
53 - II. 0～n-1中缺失的数字
　　１.二分
57.和为S的两个数字
	1.二分+双指针

【dp】
42.连续子数组的最大和  
	1.dp 
49.丑树
	1.dp

【分治】
33.二叉搜索树的后序遍历序列 
	1.分治 根据数组最后数据判断为当前的根节点，然后分别求左子树和右子树。

【二叉树】    
7.重建二叉树
    1.前序+中序 递归解决
26.树的子结构
	1.递归调用 recur
27.二叉树的镜像
    1.递归 2.栈结构
28.对称的二叉树
    1.递归  判断左右子节点 
32-1 从上到下打印二叉树-左右顺序
	1,队列  和下面的类似 
32-2 从上到下打印二叉树-层序遍历
	1.队列  2.递归
32-3 从上到下打印二叉树-左右+逆序
	1.队列 + flag 逆序标志
37.序列化二叉树
	1.序列化 二叉树-》字符串 2.反序列化 字符串-》二叉树
55-1 二叉树的深度
	1.dfs
55-2 平衡二叉树
	1.dfs flag标志
68-1 二叉搜索树的最近公共祖先
 	1.根据BST的特点 DFS
68-2 二叉树的最近公共祖先
	1.dfs 递归左右子节点
	
【栈-队列】
9.用两个栈实现队列【高频】
    1.in out 两个栈 删除的三种情况 
30.包含min函数的栈
	1.data 和 min栈 保证minValue的值的变化
59.队列的最大值
	1.两个双端队列 一个存储数据 一个存储最大值。
59.2 滑动窗口最大值 【高频】【面试之前AC】
	1.双端队列
	
【递归-dp】  
10-1.斐波那契额数列
    1.递归  2.数组 3.dp
10-2.青蛙跳台阶【高频】
    1.递归 2，记忆化
10-3 变态跳台阶
	1.f(n) = 2^(n-1)

【回溯】
12.矩阵中的路径  
	1.回溯
13.机器人的路径
    1.回溯  穷举所有路径 排除掉不符合条件的路径
34.二叉树中的合围某一值的路径 【面试之前必看】
	1.回溯算法  
38.字符串的排列 【面试之前必看】
	1.回溯算法
	
【位运算】   
15.二进制中的1的个数
    1.n&1 n>>>1 2.n&=(n-1)
64.求1+2+…+n  
    1.&&判断
    
【字符串】
50.第一个只出现一次的字符
	1.hash思想
58 - I. 翻转单词顺序 
	1.字符串 先分隔 在逆序拼接
58 - II. 左旋转字符串
	1.先逆序前半部分 在逆序后半部分 最后逆序整个。
67.把字符串转换成整数
	1.注意符号 以及溢出 
	
【堆】
40.最小的k个数
    1.堆 TOP(K)问题
41.数据流中的中位数
 	1.大顶堆 和 小顶堆 结合使用
  
【排序】
  
【dfs】
54.二叉搜索树的第K大节点
    1.左中右为有序的递增序列 右中左就位递减序列 --k  + dfs就可以


【链表】
6.从尾到头打印链表
    1.栈  2.递归  3.头插法
18.删除链表的节点
	1.迭代查找  2.快慢指针删除法  pre.next = fast.next
22.链表中倒数第K个节点
	1.快慢指针  快指针走k步 
24.反转链表
	1.背下来	
25.合并两个排序的链表
	1.前置pre + 遍历 l1 l2
35.复杂链表的复制
   1.hashMap存储Node 在依次遍历查找next 和 randomNode
52.两个链表的公共节点
   1.两个指针 谁先走完 走对方的  直到相遇。
```



## 3.数组中重复的数字  ★

### 题目描述

> 找出数组中重复的数字。
>
>
> 在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。
>
> 示例 1：
>
> 输入：
> [2, 3, 1, 0, 2, 5, 3]
> 输出：2 或 3 

### 1.HashSet

使用hashSet去重  如果添加不成功说明出现了重复的元素 返回。

```java
public int findRepeatNumber(int[] nums) {
        if(nums.length == 0 || nums.length <2){
            return -1;
        }
        Set<Integer> set = new HashSet<Integer>();
        int repeat = -1;
    // 1 2 1
        for (int num : nums) {
            if (!set.add(num)) {//添加失败 取反说明之前添加过了
                repeat = num;
                break;
            }
        }
        return repeat;
    }
```

时间复杂度:O(n)

空间复杂度:O(n)

### 2.利用下标

将数组中的元素当成下标存储到数组中去，如果没有重复的就说明无重复数 否则有重复数字

比如数组[2, 3, 1, 0, 2, 5, 3]

i = 0,  nums[0]!=0     swap [1,3,2,2,5,3]

i = 0,  nums[0]!=0     swap [3,1,2,2,5,3]

```java
	public int findRepeatNumber(int[] nums) {
        if(nums == null || nums.length == 0){
            return -1;
        }
		// 1 2 1
        int repearNum = 0;
        for(int i=0;i<nums.length;i++){
            while(nums[i]!=i){
                if(nums[i]==nums[nums[i]]){ // 相遇 返回
                    repearNum = nums[i];
                    break;
                }
                swap(i,nums[i],nums);
            }
        }
        return repearNum;
    }

    public void swap(int i,int j,int [] nums){
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
```

时间复杂度:O(n)

空间复杂度:O(1)

## 4.二维数组中的查找 ★

> 在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
>
>  
>
> 示例:
>
> 现有矩阵 matrix 如下：
>
> [
>   [1,   4,  7, 11, 15],
>   [2,   5,  8, 12, 19],
>   [3,   6,  9, 16, 22],
>   [10, 13, 14, 17, 24],
>   [18, 21, 23, 26, 30]
> ]
> 给定 target = 5，返回 true。
>
> 给定 target = 20，返回 false。

### 1.暴力破解法

thinking:两层loop 查找

时间复杂度:O(m*n)  **二维数组中的每个元素都被遍历，因此时间复杂度为二维数组的大小。**

空间复杂度:O(1)

```java
/***
    *1.暴力法
    */
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        if(matrix == null || matrix.length == 0 || matrix[0].length == 0){
            return false;
        }

        for(int i=0;i<matrix.length;i++){
            for(int j=0;j<matrix[i].length;j++){
                if(matrix[i][j] == target){
                    return true;
                }
            }
        }
        return false;
    }
```

### 2.线性查找

thinking：通过分析，可以知道 如果使用暴力破解的话 会出现重复的元素查找，因此，在一定程度上，不可缺。如果一开始从右上角开始查找 如果target小于当前数 说明不在这一列，所以，排除掉一列，如果反复，如果当前数大于target说明在这一列上，rows++。

时间复杂度:O(m+n)  访问到的下标的行最多增加 `n` 次，列最多减少 `m` 次，因此循环体最多执行 `n + m` 次。

空间复杂度:O(1)

```java
public boolean findNumberIn2DArray(int[][] matrix, int target) {
        if(matrix == null || matrix.length == 0 || matrix[0].length == 0){
            return false;
        }

        int rows = matrix.length-1,cols = matrix[0].length-1;
        int r = 0,c = cols;//右上角开始
        while(r<=rows && c >=0){
            int num = matrix[r][c];
            if(target == num){
                return true;
            }
            if(target < num){
                c--;
            }else{
                r++;
            }
        }
        return false;
    }
```

## 5.替换空格 ★

> #### [面试题05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)
>
> 难度简单8
>
> 请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。
>
> **示例 1：**
>
> ```
> 输入：s = "We are happy."
> 输出："We%20are%20happy."
> 
> ```

### 迭代

th:直接迭代就可以了 需要注意的是方法名的调用。

时间复杂度:O(n)

空间复杂度:O(n)

```java
public String replaceSpace(String s) {
        if(s.length() == 0){
            return "";
        }
        StringBuffer sb = new StringBuffer();
        for(Character ch : s.toCharArray()){
            if(ch == ' '){
                sb.append("%20");
            }else{
                sb.append(ch);
            }
        }
        return sb.toString();
    }
```

## 6.从尾到头打印链表 ⭐⭐

> 输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。
>
>  
>
> **示例 1：**
>
> ```
> 输入：head = [1,3,2]
> 输出：[2,3,1]
> ```

### 1.使用栈

th:逆序打印 我们可以将链表loop一遍，push到栈中。然后pop出 因为栈是先进后出 所有最后顺序就是逆序的顺序。 这里建议push最好是val push node的话 占用的内存空间比较大。

time: O(n) 需要遍历一遍链表

space:O(n) 需要大小为size的数组存储数据。

```java
public int[] reversePrint(ListNode head) {
        if(head == null){
            return new int[0];
        }
        Stack<ListNode>stack = new Stack<>();
        
        ListNode cur = head;
        while(cur!=null){
            stack.push(cur);
            cur = cur.next; 
        }
        int [] array = new int [stack.size()];
        int i = 0;
        while(!stack.isEmpty()){
            array[i++] = stack.pop().val;
        }
        return array;
    }
```

### 2.递归法

th: 递推阶段:每次传入head.next 以head.next ==null 为终止条件，此时返回。

回溯节点:层层回溯时，将以当前节点值加入列表。

最后转换成int数组即可。

time:O(n) 遍历链表n次

space:O(n) 系统递归需要使用O(n)的栈空间

```java
List<ListNode> list = new ArrayList<>();

    /***
      2.递归
    */
    public int[] reversePrint(ListNode head) {
      //递归
      reverse(head);
      int [] array = new int [list.size()];
    
      for(int i=0;i<list.size();i++){
          array[i] = list.get(i).val;
      }
      return array;
    }

    public void reverse(ListNode curr){
        //1.终止条件
        if(curr == null){
            return;
        }
      	//2.递归子问题
        reverse(curr.next);
        // 3.业务逻辑
        list.add(curr);
    }
```

### 3.头插法【ps】

头插法顾名思义是将节点插入到头部，在遍历原始链表时，将当前节点插入新链表的头部，使其称为第一个节点，

链表的操作需要维护后继关系，例如在某个节点node1之后插入一个节点node2,我们可以通过修改后继关系来实现。

```java
node3 = node1.next;
node2.next = node3;
node1.next = node2;
```

​	为了能将一个节点插入头部，我们引入了一个叫**头结点的辅助节点**，该节点不存储值，只是为了方便进行插入操作。不要将头结点与第一个节点混起来，第一个节点是链表中第一个真正存储值的节点。

```java
 	//头插法
    public int[] reversePrint(ListNode head) {
        ListNode node = new ListNode(-1);
        int size = 0;
        //1 while   -1 	-->1 
        //2 while   -1 	-->2   -->1
        //3 while   -1 	-->3   -->2  -->1
        while(head != null){
            ListNode memo = head.next;  //存储后继节点
            head.next = node.next;  //
            node.next = head;  //和头结点连接上
            head = memo;  //head 继续遍历下去
            size++; 
        }

        int [] arr = new int [size];
        head = node.next;
        int i = 0;
        while(head != null){
            arr[i++] = head.val;
            head = head.next;
        }
        return arr;
    }
```

## 7.重建二叉树 ★

> 输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。
>
>  
>
> 例如，给出
>
> 前序遍历 preorder = [3,9,20,15,7]
> 中序遍历 inorder = [9,3,15,20,7]
> 返回如下的二叉树：
>
>     3
>    / \
>   9  20
>     /  \
>    15   7

th:前序遍历第一个数就是root节点，而我们用map记录中序顺序，用根节点区分左右节点 递归调用。

time : O(n)

space : O(n)

```java
   	static Map<Integer,Integer> result = new HashMap<>();
    
		public static TreeNode buildTree(int[] preorder, int[] inorder) {
        for(int i=0;i<inorder.length;i++){
            result.put(inorder[i],i);
        }
        return recur(preorder,0,preorder.length-1,0);
    }

    public static TreeNode recur(int [] pre,int preL,int preR,int inL){
        if(preL > preR){
            return null;
        }
        TreeNode root = new TreeNode(pre[preL]);
        int rootIndex = result.get(root.val);
        int size = rootIndex - inL;
        root.left = recur(pre,preL+1,preL+size,inL);
        root.right = recur(pre,preL+1+size,preR,inL+size+1);
        return root;
    }
```

## 8.用两个栈实现队列 ⭐⭐

> 用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )
>
>  
>
> 示例 1：
>
> 输入：
> ["CQueue","appendTail","deleteHead","deleteHead"]
> [[],[3],[],[]]
> 输出：[null,null,3,-1]

th：用两个栈，一个栈push数据 另一个栈pop数据

输入数据 1 2 3 push进inStack 为  3 2 1 在pop到outStack栈中 为1 2 3 和输入数据顺序一样、

time：O(1)

space:O(n)

```java
 /*
    * 用两个栈，一个栈push数据 另一个栈pop数据
    输入数据 1 2 3 push进inStack 为  3 2 1 在pop到outStack栈中 为1 2 3 和输入数据顺序一样、
    */
    Stack<Integer> in;
    Stack<Integer> out;
    public Queue() {
        in = new Stack();
        out = new Stack();
    }
    
    public void appendTail(int value) {
        in.push(value);
    }
    
    public int deleteHead() {
        if(out.empty()){//out栈为null 才添加数据 tips
            while(!in.empty()){
                out.push(in.pop());
            }
        }

        if(out.empty()){
            return -1;
        }

        return out.pop();
    }
```

## 9.斐波那契数列⭐⭐

> 写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项。斐波那契数列的定义如下：
>
> F(0) = 0,   F(1) = 1
> F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
> 斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。
>
> 答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。
>
>  
>
> 示例 1：
>
> 输入：n = 2
> 输出：1
> 示例 2：
>
> 输入：n = 5
> 输出：5



### 1.使用递归

递归的使用就是将一个大问题分解成多个子问题进行递归解决。

```java
public int fib(int n) {
        if(n<=1){
            return  n;
        }

        return fib(n-1)+fib(n-2);
    }
```

问题 递归调用的过程中会出现重复计算的子问题。当递归调用栈的深度超过系统栈 就会出异常。

### 2.记忆化递归法

动态规划的思想也是将大问题化解成多个子问题，但是动态规划会将重复计算的子问题的解存储起来。这样避免了重复计算带来的性能开销。

```java
	public int fib(int n) {
        if(n<=1){
            return n;
        }

        int [] fib = new int [n+1];
 
        fib[1] = 1;
        for(int i=2;i<=n;i++){
            fib[i] = fib[i-1]+fib[i-2];
        }
        return fib[n];
    }
```

### 3.动态规划

分析一下 空间复杂度是O(n)

```java
public int Fibonacci(int n) {
    if (n <= 1)
        return n;
    int pre2 = 0, pre1 = 1;
    int fib = 0;
    for (int i = 2; i <= n; i++) {
        fib = pre2 + pre1;
        pre2 = pre1;
        pre1 = fib;
    }
    return fib;
}
```

空间复杂度由O(n)降低到O(1)

### 4.循环求余法

```java
class Solution {
    public int fib(int n) {
        int a = 0, b = 1, sum;
        for(int i = 0; i < n; i++){
            sum = (a + b) % 1000000007;
            a = b;
            b = sum;
        }
        return a;
    }
}
```

时间复杂度 O(N) ： 计算 f(n) 需循环 n 次，每轮循环内计算操作使用 O(1) 。
空间复杂度 O(1) ： 几个标志变量使用常数大小的额外空间。

## 10.青蛙跳台阶问题 ⭐⭐

> 一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。
>
> 答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。
>
> 示例 1：
>
> 输入：n = 2
> 输出：2
> 示例 2：
>
> 输入：n = 7
> 输出：21

time : O(n)

space:O(1)

```java
public int numWays(int n) {
       int a = 1, b = 1, sum;
        for(int i = 0; i < n; i++){
            sum = (a + b) % 1000000007;
            a = b;
            b = sum;
        }
        return a;
    }
```

## 10-2 变态跳台阶 ★

> 一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

变态跳台阶问题，是斐波那契额数列 青蛙跳台阶  跳楼梯问题的变形。

假设当前在3台阶，那么可能从0级跳到3台阶 1台阶到3台阶 2台阶到3台阶 3台阶到3台阶。

f(3) = f(1)+f(2)+f(3) 

f(1) = 1

f(2) =  2

f(3) = 1+2+1 

### 1.暴力解

```java
	public int JumpFloorII(int target) {
       if(target == 0 || target == 1) {
           return 1;
       }
        int [] f = new int [target+1];
        f[0] = f[1] = 1;
        for(int i=2;i<=target;i++){
            for(int j=0;j<i;j++){
                f[i] += f[j];
            }
        }
        return f[target];
    }
```

### 2.递归

f(n) = 2^(n-1) 找到规律

```java
	public int JumpFloorII(int target) {
        if(target < 0){
            return -1;
        }else if(target == 1){
            return 1;
        }else{
            return 2*JumpFloorII(target-1);
        }
    }
```

## 11. 旋转数组的最小数字 ⭐⭐

> 难度简单35
>
> 把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 `[3,4,5,1,2]` 为 `[1,2,3,4,5]` 的一个旋转，该数组的最小值为1。  
>
> **示例 1：**
>
> ```
> 输入：[3,4,5,1,2]
> 输出：1
>
> ```
>
> **示例 2：**
>
> ```
> 输入：[2,2,2,0,1]
> 输出：0
> ```

th:这道题主要的关键点在于找到递增后减小的位置，使用二分可以解决。

有三种情况

1.当nums[mid]>nums[right] 说明 一定在右边  ps : 1 3 4 5 0 2 mid>right   left = mid+1;

2.当nums[mid] = nums[right]  说明出现了重复的元素  这个时候要缩小范围  right = right-1

3.当nums[mid] < nums[right]  说明 一定在左边  right = mid;

time:O(logN)  当出现 1 1 1 1 会退化到O(n)

space:O(1)

```java
 public int minArray(int[] numbers) {
        int length = numbers.length;
        if(length == 0){
            return -1;
        }

        int left = 0,right = length-1;
      
        while(left < right){
              int mid = (left+right) >>>1;//获取中点位置
            // 3 4 5 1 2 
            if(numbers[mid]>numbers[right]){
                //右边
                left = mid+1;
            }else if(numbers[mid] == numbers[right]){
                //缩小范围
                right = right-1;
            }else{
                //numbers[mid] < number[right]
                right = mid;
            }
        }
        return numbers[left];
    }
```

## 12.矩阵中的路径

> #### [面试题12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)
>
> 难度中等72
>
> 请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。
>
> [["a","**b**","c","e"],
> ["s","**f**","**c**","s"],
> ["a","d","**e**","e"]]
>
> 但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。
>
>  
>
> **示例 1：**
>
> ```
> 输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
> 输出：true
> ```

### dfs

路径问题 一般直接深度优先搜素查找所有的路径。这就是回溯算法。先选取一个点看相等吗 不相等 回溯操作。

时间复杂度 比较高。

```java
	//dfs
    public boolean exist(char[][] board, String word) {
        char [] words = word.toCharArray();
        for(int i=0;i<board.length;i++){
            for(int j=0;j<board[0].length;j++){
                if(dfs(board,words,i,j,0)){
                    return true;
                }
            }
        }
        return false;
    }

    private boolean dfs(char [][] board, char [] words,int i,int j,int k){
        if(i<0 || j < 0 || i>=board.length || j>=board[0].length){//边界判断
            return false;
        }
        if(board[i][j]!=words[k]){//不符合
            return false;
        }
        if(words.length-1 == k){
            return true;
        }
        //决策
        char tmp = board[i][j];
        board[i][j] = '*';
        //上下左右穷举
        boolean res = dfs(board,words,i+1,j,k+1) || dfs(board,words,i-1,j,k+1)  || dfs(board,words,i,j+1,k+1) || dfs(board,words,i,j-1,k+1);
        board[i][j] = tmp;//取消决策
        return res;
    }
```

## 13.机器人的运动范围

> #### [面试题13. 机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)
>
> 难度中等96
>
> 地上有一个m行n列的方格，从坐标 `[0,0]` 到坐标 `[m-1,n-1]` 。一个机器人从坐标 `[0, 0] `的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？
>
>  
>
> **示例 1：**
>
> ```
> 输入：m = 2, n = 3, k = 1
> 输出：3
> ```
>
> **示例 2：**
>
> ```
> 输入：m = 3, n = 1, k = 0
> 输出：1
> ```

### 回溯

用visited记录当前是否访问过，终止条件 i j条件 

```java
	int m = 0;//行
    int n = 0;//列
    int k = 0;//阈值
    boolean [][] visited;//记录是否访问过 
    public int movingCount(int m, int n, int k) {
        this.m = m;
        this.n = n;
        this.k = k;
        visited = new boolean [m][n];
        return count(0,0);
    }

    private int count(int i,int j){
        if(i<0||j<0|| i>=m || j >= n|| visited[i][j] || i/10+i%10+j/10+j%10>k){
            return 0;
        }
        visited[i][j] = true;
        return 1+count(i+1,j) + count(i-1,j)+count(i,j+1)+count(i,j-1);
    }
```

## 14- I. 剪绳子

> #### [面试题14- I. 剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)
>
> 难度中等45
>
> 给你一根长度为 `n` 的绳子，请把绳子剪成整数长度的 `m` 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 `k[0],k[1]...k[m-1]` 。请问 `k[0]*k[1]*...*k[m-1]` 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。
>
> **示例 1：**
>
> ```
> 输入: 2
> 输出: 1
> 解释: 2 = 1 + 1, 1 × 1 = 1
> ```
>
> **示例 2:**
>
> ```
> 输入: 10
> 输出: 36
> 解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
> ```

```java
public int cuttingRope(int n) {
        if(n<=3){
            return n-1;
        }
        int a = n/3;
        int b = n%3;
        if(b==0){
            return (int)Math.pow(3,a);
        } 
        if(b==1){
            return (int)Math.pow(3,a-1)*4;
        }
        return (int)Math.pow(3,a)*2;
    }
```

![img](https://img-blog.csdnimg.cn/20200510131840682.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMTEyMjM4,size_16,color_FFFFFF,t_70)

## 14- II 剪绳子

> #### [面试题14- II. 剪绳子 II](https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/)
>
> 难度中等16
>
> 给你一根长度为 `n` 的绳子，请把绳子剪成整数长度的 `m` 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 `k[0],k[1]...k[m]` 。请问 `k[0]*k[1]*...*k[m]` 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。
>
> 答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。
>
>  
>
> **示例 1：**
>
> ```
> 输入: 2
> 输出: 1
> 解释: 2 = 1 + 1, 1 × 1 = 1
> ```
>
> **示例 2:**
>
> ```
> 输入: 10
> 输出: 36
> 解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
> ```

```java
	public int cuttingRope(int n) {
        if(n==2||n==3)
            return n-1;       
        int mod = 1000000007;
        long res = 1;
        while(n>4){
            res*=3;
            res%=mod;
            n-=3;
        }
        return (int)(res*n%mod);
    }
```



## 15.二进制中1的个数 ⭐⭐

> #### 难度简单14收藏分享切换为英文关注反馈
>
> 请实现一个函数，输入一个整数，输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。
>
> **示例 1：**
>
> ```
> 输入：00000000000000000000000000001011
> 输出：3
> 解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。
>
> ```
>
> **示例 2：**
>
> ```
> 输入：00000000000000000000000010000000
> 输出：1
> 解释：输入的二进制串 00000000000000000000000010000000 中，共有一位为 '1'。
>
> ```
>
> **示例 3：**
>
> ```
> 输入：11111111111111111111111111111101
> 输出：31
> 解释：输入的二进制串 11111111111111111111111111111101 中，共有 31 位为 '1'。
> ```

### 1.逐位判断

th：使用位运算符将数字每次右移一次 & 1如果为1 就result++ 

time:O(n)

space:O(1)

```java
public int hammingWeight(int n) {
        int result = 0;
        while(n!=0){
            result+= n & 1;
            n >>>= 1;
        }
        return result;
    }
```

### 2.巧用n &=(n-1)

th:假设 n = 3      n= 0101   n-1 = 0100   n&=(n-1)   n = 0100  消去一个1 result++  再次循环  

n -1 = 0000     n&=(n-1)  n=0 退出循环  result = 2 

time:O(n)

space:O(1)

```java
public int hammingWeight(int n) {
        //1.巧用 n&=(n-1)
        int result = 0;

        while(n!=0){
            result++;
            n &= (n-1);
        }
        return result;
    }
```



## 16.数值的整数次方

> [面试题](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)
>
> 难度中等14
>
> 实现函数double Power(double base, int exponent)，求base的exponent次方。不得使用库函数，同时不需要考虑大数问题。
>
> **示例 1:**
>
> ```
> 输入: 2.00000, 10
> 输出: 1024.00000
>
> ```
>
> **示例 2:**
>
> ```
> 输入: 2.10000, 3
> 输出: 9.26100
>
> ```
>
> **示例 3:**
>
> ```
> 输入: 2.00000, -2
> 输出: 0.25000
> 解释: 2-2 = 1/22 = 1/4 = 0.25
> ```

### 1.暴力

如果n<0 x= 1/x n = -n 遍历求解 result = result * x;

time：O(n)

space:O(1)

```java
public double myPow(double x, int n) {
        if(n<0){//如果负数 1/x  n = -n
            x = 1/x;
            n = -n;
        }

        double result = 1.0f;
        for(int i = 0;i<n;i++){
            result = result *x;
        }
        return result;
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 2.快速幂算法

time : O(logn)

space:O(logn)

```java
public double myPow(double x, int n) {
        if(n<0){//如果负数 1/x  n = -n
            x = 1/x;
            n = -n;
        }

        return quickPow(x,n);
    }

    public double quickPow(double x,int n){
        if(n == 0){
            return 1.0f;
        }

        double half = quickPow(x,n/2);

        if((n & 1) == 0){
            return  half * half;
        }else{
            return  half * half * x;
        }
    }
```

### 3.快速幂算法-循环版

time : O(logN)

space:O(1)

```java
public double myPow(double x, int n) {
        if(n == 0){
            return 1;
        }

        if(n < 0){
            x = 1/x;
            n = -n;
        }

        double result = 1;

        for(int i = n;i>=0;i/=2){
            if((i & 1) == 1){
                result *= x;
            }
            x *= x;
        }

        return result;
    }
```



## 17. 打印从1到最大的n位数

> 难度简单10
>
> 输入数字 `n`，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。
>
> **示例 1:**
>
> ```java
> 输入: n = 1
> 输出: [1,2,3,4,5,6,7,8,9]
> ```

直接用Math.pow(10,n)  计算出大小 比如n =  2 Math.pow(10,2)  100 -1  = 99

time:O(n)

space:O(n)

```java
public int[] printNumbers(int n) {
        if(n<=0){
            return new int [0];
        }
        int [] arr = new int [(int)Math.pow(10,n) - 1];
        for(int i=0;i<arr.length;i++){
            arr[i] = i+1;
        }
        
        return arr;
    }
```





## 18. 删除链表的节点

> 难度简单12收藏分享切换为英文关注反馈
>
> 给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。
>
> 返回删除后的链表的头节点。
>
> **注意：**此题对比原题有改动
>
> **示例 1:**
>
> ```
> 输入: head = [4,5,1,9], val = 5
> 输出: [4,1,9]
> 解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.
>
> ```
>
> **示例 2:**
>
> ```
> 输入: head = [4,5,1,9], val = 1
> 输出: [4,5,9]
> 解释: 给定你链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9.
> ```

### 1.遍历法

th:通过loop遍历 找到等于val的节点 将上一个节点和要删除节点的next节点相连接。

time:O(n)

time:O(1)

```java
public ListNode deleteNode(ListNode head, int val) {
        if(head == null || head.next == null){
            return null;
        }

        //遍历查找到Node.val = val的结点
        ListNode root  = head;

        //如果是头结点就是要删除的节点 直接用root.next 指向根节点  返回即可
        if(root.val == val){
            head = root.next;
            return head;
        }

        while(root.next != null){
            if(root.next.val == val){
                root.next = root.next.next;
                return head;
            }
            root = root.next;
        }

        return head;
    }
```

### 2.双指针

th:一共两种情况 1. 头结点就是删除的节点 直接返回head.next 即可。

2.删除的节点在链表中某个位置。 一个pre节点  和 一个 cur节点  cur节点比pre节点快一步 找到删除即可。

time:O(n)

space:O(1)

```java
public ListNode deleteNode(ListNode head, int val) {
        //如果是head节点 直接返回
        if(head.val == val){
            return head.next;
        }

        ListNode pre = head, cur = head.next;
        //遍历寻找
        while(cur != null && cur.val != val){
            pre = cur;
            cur = cur.next;
        }

        if(cur!=null){
            pre.next = cur.next;
        }
        return head;
    }
```

## 19.正则表达式匹配

> #### [面试题19. 正则表达式匹配](https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof/)
>
> 难度困难53
>
> 请实现一个函数用来匹配包含`'. '`和`'*'`的正则表达式。模式中的字符`'.'`表示任意一个字符，而`'*'`表示它前面的字符可以出现任意次（含0次）。在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串`"aaa"`与模式`"a.a"`和`"ab*ac*a"`匹配，但与`"aa.a"`和`"ab*a"`均不匹配。
>
> **示例 1:**
>
> ```
> 输入:
> s = "aa"
> p = "a"
> 输出: false
> 解释: "a" 无法匹配 "aa" 整个字符串。
> ```







## 20.表示数值的字符串

> #### [面试题20. 表示数值的字符串](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)
>
> 难度中等9
>
> 请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100"、"5e2"、"-123"、"3.1416"、"0123"及"-1E-16"都表示数值，但"12e"、"1a3.14"、"1.2.3"、"+-5"及"12e+5.4"都不是。
>
>  
>
> 注意：本题与主站 65 题相同：<https://leetcode-cn.com/problems/valid-number/>

正则表达式

```
[] : 字符集合
() : 分组
?  : 重复0-1次
+  ：重复1-n次
*  : 重复0-n次
.  : 任意字符
\\. :转义后的.
\\d :数字
```



```java
public boolean isNumber(String s) {
        if (s == null || s.length() == 0)
            return false;
        String str="^[+|-]?((\\d+\\.?)|(\\d*\\.\\d+))([E|e][+|-]?\\d+)?$";
        return s.trim().matches(str);
    }
```

## 21. 调整数组顺序使奇数位于偶数前面  ⭐⭐

> 难度简单12
>
> 输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。
>
>  
>
> **示例：**
>
> ```
> 输入：nums = [1,2,3,4]
> 输出：[1,3,2,4] 
> 注：[3,1,2,4] 也是正确的答案之一。
> ```

### 1.双指针

思路:i指向前边，j指向后边 i从前往后移动 当不是奇数的时候就跳出  j从后往前移动不是偶数的时候跳出。i 和 j下标所在的位置数据进行交换。终止条件是i<j 多次交换以后 前边的数据就是奇数，后边的数据是偶数。

time :O(n)

space:O(1)

```java
public int[] exchange(int[] nums) {
        //参数判断
        if(nums.length == 0 || nums == null){
            return new int [0];
        }

        int i = 0,j = nums.length-1,temp;
        //while
        while(i<j){
            //i++ 查找不是奇数跳出
            while(i<j && ((nums[i] & 1) == 1)) i++;
            //j-- 查找不是偶数的跳出
            while(i<j && ((nums[j] & 1) == 0)) j--; 
            //交换
            temp = nums[i];  
            nums[i] = nums[j];
            nums[j] = temp;
        }
        return nums;
    }
```

### 2.需要额外的空间

```java
	public void reOrderArray(int [] array) {
        if(array == null || array.length == 0){
            return;
        }
        int [] data = new int [array.length];
        int m = 0,n = 0;
        for(int i=0;i<array.length;i++){
            if((array[i]&1)==1){
                array[m++] = array[i];
            }else if((array[i]&1)==0){
                data[n++] = array[i];
            }
        }
        
        for(int i=0;i<n;i++){
            array[m++] = data[i];
        }
    }
```



## 22. 链表中倒数第k个节点  ⭐⭐

> #### [面试题22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)
>
> 难度简单23收藏分享切换为英文关注反馈
>
> 输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。例如，一个链表有6个节点，从头节点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的节点。
>
>  
>
> **示例：**
>
> ```
> 给定一个链表: 1->2->3->4->5, 和 k = 2.
>
> 返回链表 4->5.
> ```

### 1.双指针-快慢指针

思路:一般这种题 我们可以用双指针中的快慢指针进行解决，首先定义一个快指针fastNode 先让fastNode走k步，然后定义一个慢指针 慢指针和快指针同时走 当fastNode走到指针头就停止，这时slowNode就在倒数第k个节点

time:O(n)

space：O(1)

```java
//定义一个fastNode 先走k步 在定义一个slowNode slowNode和fastNode同时走 当fastNode走到头
    //此时slowNode就走到了倒数第k个节点 
    public ListNode getKthFromEnd(ListNode head, int k) {
        if(head == null || k == 0){
            return null;
        }

        ListNode fastNode = head;
        for(int i=0;i<k;i++){
            fastNode = fastNode.next;
        }

        ListNode slowNode = head;
        while(fastNode != null){
            fastNode = fastNode.next;
            slowNode = slowNode.next;
        }
        return slowNode;
    }
```

## 24.反转链表 ⭐⭐

> #### [面试题24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)
>
> 难度简单28收藏分享切换为英文关注反馈
>
> 定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。
>
>  
>
> **示例:**
>
> ```
> 输入: 1->2->3->4->5->NULL
> 输出: 5->4->3->2->1->NULL
> ```

思路：链表的反转，通常需要定义一个前置节点 遍历链表中所有结点。

time:O(n)

space:O(1)

```java
//思路 每次遍历 
    public ListNode reverseList(ListNode head) {
        if(head == null){
            return null;
        }
        ListNode cur = head,pre = null;
        while(cur!=null){
            ListNode next = cur.next;//设置当当前节点next 赋值到next上
            cur.next = pre;//cur.next = null
            pre = cur; //当前节点指向pre
            cur = next;//指向下一个节点
        }
        return pre;
    }

   Node next = cur.next;
   cur.next = pre;
   pre = cur;
   cur = next;pre
```



## 25.合并两个排序的链表  ⭐⭐

> #### [面试题25. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)
>
> 难度简单17
>
> 输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。
>
> **示例1：**
>
> ```
> 输入：1->2->4, 1->3->4
> 输出：1->1->2->3->4->4
> ```

### 1.迭代

时间复杂度:O(m+n)

空间复杂度:O(1)

```java
 public ListNode mergeTwoLists(ListNode l1, ListNode l2) {

        //头结点
        ListNode cur = new ListNode(-1);
        ListNode head = cur;
        while(l1 != null && l2 != null){
            //l1链表
            if(l1.val < l2.val){
                cur.next = l1;
                l1 = l1.next;//l1链表移动
            }else{
                cur.next = l2;
                l2 = l2.next;//l2链表移动
            }
            cur = cur.next;
        }

        cur.next = (l1 !=null  ? l1 : l2);
        return head.next;
    }
```

## 26.树的子结构 ★

> #### [面试题26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)
>
> 难度中等37
>
> 输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)
>
> B是A的子结构， 即 A中有出现和B相同的结构和节点值。
>
> 例如:
> 给定的树 A:
>
> `     3    / \   4   5  / \ 1   2`
> 给定的树 B：
>
> `   4   / 1`
> 返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。
>
> **示例 1：**
>
> ```
> 输入：A = [1,2,3], B = [3,1]
> 输出：false
> ```

思路:

//1.若A的根节点和B的根节点相同，则递归调用 

//  	a.终止条件 b == null 说明b遍历完毕 返回true
//  	b.a == null a.val != b.val 说明a树遍历结束没有找到b的开始根节点
// 	 c.递归调用a的左节点和b的左节点 或者a的右节点 和 b的右节点
//2.调用A的做节点和B子树比较  重复上述步骤
//3.A的右节点和B子树比较  重复1

时间复杂度:O(MN)

空间复杂度:O(M)

```java
//1.若A的根节点和B的根节点相同，则递归调用 
    //  a.终止条件 b == null 说明b遍历完毕 返回true
    //  b.a == null a.val != b.val 说明a树遍历结束没有找到b的开始根节点
    //  c.递归调用a的左节点和b的左节点 或者a的右节点 和 b的右节点
    //2.调用A的做节点和B子树比较  重复上述步骤
    //3.A的右节点和B子树比较  重复1
    public boolean isSubStructure(TreeNode A, TreeNode B) {
        return (A != null && B != null ) && (recur(A,B) || isSubStructure(A.left,B) || isSubStructure(A.right,B));
    }

    private boolean recur(TreeNode a,TreeNode b){
        if(b == null) return true;
        if(a == null || a.val != b.val) return false;
        return recur(a.left,b.left) && recur(a.right,b.right);
    }
```



## 27.二叉树的镜像  ⭐

> #### [面试题27. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)
>
> 难度简单15
>
> 请完成一个函数，输入一个二叉树，该函数输出它的镜像。
>
> 例如输入：
>
> `     4   /   \  2     7 / \   / \1   3 6   9`
> 镜像输出：
>
> `     4   /   \  7     2 / \   / \9   6 3   1`
>
>  
>
> **示例 1：**
>
> ```
> 输入：root = [4,2,7,1,3,6,9]
> 输出：[4,7,2,9,6,3,1]
> ```

### 1.递归

时间复杂度:O(n) 建立二叉树的所有结点遍历一遍

空间复杂度:O(n) 最坏情况下 二叉树退化成链表，需要n个存储空间。

```java
	//1.暂存左节点
    //2.递归遍历右节点。
    //  a.右节点不为空继续下一层
    //  b.右节点为空 直接作为根节点的左节点。
    //3.递归遍历左节点
    //  a.左节点不为空继续下一层
    //  b.右节点为空 直接作为根节点的右节点。
    // 如此反复就可以修改。 
    public TreeNode mirrorTree(TreeNode root) {
        //递归
        if(root == null ) return null;
        TreeNode temp = root.left;
        root.left = mirrorTree(root.right);
        root.right = mirrorTree(temp);
        return root;
    }
```

### 2.使用栈

时间复杂度:O(n)  遍历一下结点的个数

空间复杂度:O(n)  存储所有结点的次数

```java
	//1.根节点存储到stack中
    //2.当stack 不为null 将根节点pop出来。
    //  a.如果root.left不为null stack.push -> root.left
    //  b.如果root.right不为null stack.push -> root.right
    //3.暂存left节点 
    //  a.交换左右节点 依次循环遍历。
    //返回根节点
    public TreeNode mirrorTree(TreeNode root) {
        if(root == null){
            return null;
        }

        Stack<TreeNode> stack = new Stack();
        stack.push(root);

        while(!stack.isEmpty()){
            TreeNode root2 = stack.pop();
            if(root2.left != null) stack.push(root2.left);
            if(root2.right != null) stack.push(root2.right);
            TreeNode tmp = root2.left;
            root2.left = root2.right;
            root2.right = tmp;
        }
        return root;
    }
```

## 28.对称的二叉树  ⭐

> #### [面试题28. 对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)
>
> 难度简单25
>
> 请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。
>
> 例如，二叉树 [1,2,2,3,4,4,3] 是对称的。
>
> `    1   / \  2   2 / \ / \3  4 4  3`
> 但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:
>
> `    1   / \  2   2   \   \   3    3`
>
>  
>
> **示例 1：**
>
> ```
> 输入：root = [1,2,2,3,4,4,3]
> 输出：true
> ```

### 1.递归

时间复杂度:O(n)

空间复杂度:O(n)  最差情况下 二叉树退化成链表，系统使用O(n)大小的栈空间

```java
// 递归
    //1.如果root == null 返回 true
    //2.否则调用左右节点递归遍历
    //  a.root.left == null && root.right == null 返回true
    //  b.root.left == null || root.right == null || root.left.val != root.right.val  返回false
    //  c.递归下一层 
    public boolean isSymmetric(TreeNode root) {
       return  (root == null ) ? true : recur(root.left,root.right);
    }

    public boolean recur(TreeNode left,TreeNode right){
        if(left == null && right == null)  return true;
        if(left == null || right == null || left.val != right.val) return false;
        return recur(left.left,right.right) && recur(left.right,right.left);
    }
```

## 29.顺时针打印矩阵

> #### [面试题29. 顺时针打印矩阵](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)
>
> 难度简单25
>
> 输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。
>
>  
>
> **示例 1：**
>
> ```
> 输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
> 输出：[1,2,3,6,9,8,7,4,5]
>
> ```
>
> **示例 2：**
>
> ```
> 输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
> 输出：[1,2,3,4,8,12,11,10,9,5,6,7]
> ```

时间复杂度:O(m*n)

空间复杂度:O(1)

```java
public int[] spiralOrder(int[][] matrix) {
        //由外向内  左-》右  上-》下  右-》左  下-》上
        if(matrix.length == 0) return new int [0];
        int l = 0,r = matrix[0].length-1,t = 0,b = matrix.length - 1,x = 0;
        int [] arr = new int [(r+1) * (b+1)];
        while(true){
            for(int i = l; i <= r;i++) arr[x++] = matrix[t][i]; //left to right
            if(++t > b) break;
            for(int i = t; i <= b ;i++) arr[x++] = matrix[i][r]; // top to bottom
            if(l > --r) break;
            for(int i = r; i>= l ;i--) arr[x++] = matrix[b][i]; // right to left
            if(t > --b) break;
            for(int i = b;i >= t;i--) arr[x++] = matrix[i][l];//buttom to top
            if(++l > r) break;
        }

        return arr;
    }
```

## 30.包含min函数的栈 ⭐⭐

> #### [面试题30. 包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)
>
> 难度简单12
>
> 定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。
>
>  
>
> **示例:**
>
> ```
> MinStack minStack = new MinStack();
> minStack.push(-2);
> minStack.push(0);
> minStack.push(-3);
> minStack.min();   --> 返回 -3.
> minStack.pop();
> minStack.top();      --> 返回 0.
> minStack.min();   --> 返回 -2.
> ```

思路:一个数据栈 一个最小值栈 在push的时候，将最小值存储到最小值栈中，pop的时候  直接pop出 当最小值栈中数据为空 将最大值添加进去。

时间复杂度:o(1)

空间复杂度:O(n)

```java
	Stack<Integer> dataStack;
    Stack<Integer> minStack;
    int minValue;
    /** initialize your data structure here. */
    public MinStack() {
        dataStack = new Stack();
        minStack = new Stack();
        minValue = Integer.MAX_VALUE;
    }   
    public void push(int x) {
        dataStack.push(x);
        minValue = Math.min(x,minValue);
        minStack.push(minValue);
    }
    public void pop() {
        dataStack.pop();
        minStack.pop();
        minValue = minStack.isEmpty() ? Integer.MAX_VALUE : minStack.peek();
    }
    
    public int top() {
        return dataStack.peek();
    }
    
    public int min() {
        return minValue;
    }
```

## 31.栈的压入、弹出序列

> #### [面试题31. 栈的压入、弹出序列](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)
>
> 难度中等27
>
> 输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。
>
>  
>
> **示例 1：**
>
> ```
> 输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
> 输出：true
> 解释：我们可以按以下顺序执行：
> push(1), push(2), push(3), push(4), pop() -> 4,
> push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
>
> ```
>
> **示例 2：**
>
> ```
> 输入：pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
> 输出：false
> 解释：1 不能在 2 之前弹出。
> ```

th:用栈模拟

time:O(n)

space：O(n)

```java
//用stack存储pushed的数据 poped pop 如果相等stack.pop
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        Stack<Integer> stack = new Stack();
        int index = 0;
        //遍历pushed
        for(int i = 0,len = pushed.length ; i < len;i++){ 
            stack.push(pushed[i]);
            //如果pushed 和  popped 相等  stack pop
            while(!stack.isEmpty() && index < len && stack.peek() == popped[index]){
                stack.pop();
                index++;
            }
        }
        //如果栈为null 是 
        return stack.isEmpty();
    }
```

## 32-1 从上到下打印二叉树 ⭐

> #### [面试题32 - I. 从上到下打印二叉树](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)
>
> 难度中等10
>
> 从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。
>
>  
>
> 例如:
> 给定二叉树: `[3,9,20,null,null,15,7]`,
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
>
> ```
>
> 返回：
>
> ```
> [3,9,20,15,7]
> ```

th：一个队列存储遍历的节点，先存储root结点，从队列中取出来，如果不为null 将左子节点 和右子节点分别存储起来。依次循环遍历。先存储左子节点 在存储有子节点 。层序遍历。

time:O(n)

space:O(n)

```java
	//一个队列存储结点，
    //list存储值 
    public int[] levelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        List<Integer> list = new ArrayList<>();
        queue.add(root);
        while(!queue.isEmpty()){
            int cnt = queue.size();
            while(cnt-- > 0){
                TreeNode t = queue.poll();
                if(t == null){
                    continue;
                }
                list.add(t.val);
                queue.add(t.left);//左子节点
                queue.add(t.right);//右子节点
            }
        }
        int [] ret = new int[list.size()];
        for(int i=0;i<list.size();i++){
            ret[i] = list.get(i);
        }
        return ret;
    }
```

## 32-2 从上到下打印二叉树 II ⭐

> #### [面试题32 - II. 从上到下打印二叉树 II](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)
>
> 难度简单17
>
> 从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。
>
>  
>
> 例如:
> 给定二叉树: `[3,9,20,null,null,15,7]`,
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
>
> ```
>
> 返回其层次遍历结果：
>
> ```
> [
>   [3],
>   [9,20],
>   [15,7]
> ]
> ```

```java
	//递归
    List<List<Integer>> list = new ArrayList();

    public List<List<Integer>> levelOrder(TreeNode root) {
        recur(root,0);
        return list;
    }

    public void recur(TreeNode root,int k){
        //终止条件
        if(root != null){
            if(list.size()<=k){
                list.add(new ArrayList());
            }
            list.get(k).add(root.val);
            recur(root.left,k+1);
            recur(root.right,k+1);
        }
    }
```

### 2.队列+迭代

```java
public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        //添加根节点
        queue.add(root);
        while(!queue.isEmpty()){
            List<Integer> list = new ArrayList<>();
            int cnt = queue.size();
            while(cnt-- > 0){
                TreeNode t = queue.poll();//弹出首节点
                if(t == null){
                    continue;
                }
                list.add(t.val);
                queue.add(t.left);
                queue.add(t.right);
            }
            if(list.size() != 0){
                result.add(list);
            }
        }
        return result;
    }
```



## 32-3 从上到下打印二叉树 III ⭐

> #### [面试题32 - III. 从上到下打印二叉树 III](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)
>
> 难度中等15
>
> 请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。
>
>  
>
> 例如:
> 给定二叉树: `[3,9,20,null,null,15,7]`,
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
>
> ```
>
> 返回其层次遍历结果：
>
> ```
> [
>   [3],
>   [20,9],
>   [15,7]
> ]
> ```

思路:其实和前边的相同 添加一个标志位 第一次不需要反转，第二次需要反转，第三次不需要反转，依序就可以反转。

```java
public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        boolean reverse = false;//设置是否需要反转
        while(!queue.isEmpty()){
            List<Integer> list = new ArrayList<>();
            int cnt = queue.size();
            while(cnt-- > 0){
                TreeNode t = queue.poll();
                if(t == null){
                    continue;
                }
                list.add(t.val);
                queue.add(t.left);
                queue.add(t.right);
            }
            if(reverse){
                Collections.reverse(list);
            }
            reverse = !reverse;
            if(list.size() != 0)
                result.add(list);
        }
        return result;
    }
```

## 33.二叉搜索树的后序遍历序列 ★

> #### [面试题33. 二叉搜索树的后序遍历序列](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)
>
> 难度中等39
>
> 输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 `true`，否则返回 `false`。假设输入的数组的任意两个数字都互不相同。
>
>  
>
> 参考以下这颗二叉搜索树：
>
> ```
>      5
>     / \
>    2   6
>   / \
>  1   3
> ```
>
> **示例 1：**
>
> ```
> 输入: [1,6,3,2,5]
> 输出: false
> ```
>
> **示例 2：**
>
> ```
> 输入: [1,3,2,6,5]
> 输出: true
> ```

### 1.递归-分治

时间复杂度:O(n^2) 每次调用 recur(i,j) 减去一个根节点，因此递归占用 O(N) ；最差情况下（即当树退化为链表），每轮递归都需遍历树所有节点，占用 O(N)

空间复杂度 O(N) ： 最差情况下（即当树退化为链表），递归深度将达到 NN 。

```java
//递归 --分治
    //后序遍历 左右根 特点左子树节点的值均小于根节点  右子树节点的值均大于根节点
    //由此可知 数组中最后一个元素就是跟节点。通过两次for 找到第一个大于root.val的值
    //将数组划分成 小于根节点的  和 大于根节点的 两部分，通过递归调用 就可以逐一比较。
    public boolean verifyPostorder(int[] postorder) {
        if(postorder == null || postorder.length == 0){
            return true;
        }
        return verify(postorder,0,postorder.length-1);
    }

    public boolean verify(int [] postorder,int first,int last){
        if(last-first<=1){ //如果数组中只有一个元素 说明是有序的。
            return true;
        }
        int curIndex = first;
        int rootVal = postorder[last];//根节点
        //找到第一个大于根节点的值 下标
        while(curIndex < last && postorder[curIndex]<=rootVal ){
            curIndex++;
        }

        for(int i = curIndex;i<last;i++){
            //如果右子节点小于根节点 不是BST
            if(postorder[i]<rootVal){
                return false;
            }
        }
        return verify(postorder,first,curIndex-1) && verify(postorder,curIndex,last-1);
    }
```

## 34.二叉树中和为某一值的路径 ⭐⭐

> #### [面试题34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)
>
> 难度中等27
>
> 输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。
>
>  
>
> **示例:**
> 给定如下二叉树，以及目标和 `sum = 22`，
>
> ```
>               5
>              / \
>             4   8
>            /   / \
>           11  13  4
>          /  \    / \
>         7    2  5   1
>
> ```
>
> 返回:
>
> ```
> [
>    [5,4,11,2],
>    [5,8,4,5]
> ]
> ```



```java
	//回溯 
    // 递归解决 终止条件节点为null 返回 
    //  当target=0 并且左右子节点都为null的时候 进行数据添加。
    //  否则的话 直接递归左右子节点。
    List<List<Integer>> ret = new ArrayList<>();

    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        pathSum(root,sum,new ArrayList());
        return ret;
    }

    public void pathSum(TreeNode root,int target,List<Integer> path){
        if(root == null){
            return;
        }
        path.add(root.val);
        target -= root.val;
        if(target == 0 && root.left == null && root.right == null){
         //这里为什么要创建一个List 因为如果直接添加path.是添加的引用，后期的修改会影响path的数据
            ret.add(new ArrayList<>(path));
        }else{
            pathSum(root.left,target,path);
            pathSum(root.right,target,path);
        }
        //回溯，状态重置，只添加不删除就是错的路径了，因为整个递归过程用的是同一个收集器path
        path.remove(path.size()-1);
    }
    
```

## 35.复杂链表的复制 ★

> #### [面试题35. 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)
>
> 难度中等33
>
> 请实现 `copyRandomList` 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 `next` 指针指向下一个节点，还有一个 `random` 指针指向链表中的任意节点或者 `null`。
>
>  
>
> **示例 1：**
>
> ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e1.png)
>
> ```
> 输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
> 输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
>
> ```
>
> **示例 2：**
>
> ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e2.png)
>
> ```
> 输入：head = [[1,1],[2,1]]
> 输出：[[1,1],[2,1]]
>
> ```
>
> **示例 3：**
>
> **![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e3.png)**
>
> ```
> 输入：head = [[3,null],[3,0],[3,null]]
> 输出：[[3,null],[3,0],[3,null]]
>
> ```
>
> **示例 4：**
>
> ```
> 输入：head = []
> 输出：[]
> 解释：给定的链表为空（空指针），因此返回 null。
> ```



```java
	public Node copyRandomList(Node head) {
        if(head == null)  return null;
        Node node = head;
        Map<Node,Node> map = new HashMap<Node,Node>();
        //1.loop copy all node in map
        while(node!=null){
            map.put(node,new Node(node.val));
            node = node.next;
        }

        node = head;
        //2.loop assign next and random pointer
        while(node!=null){
            map.get(node).next = map.get(node.next);//next 
            map.get(node).random = map.get(node.random);
            node = node.next;
        }
        return map.get(head);
    }
```

## 36. 二叉搜索树与双向链表

> 难度中等31
>
> 输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。
>
>  
>
> 为了让您更好地理解问题，以下面的二叉搜索树为例：
>
>  
>
> ![img](https://assets.leetcode.com/uploads/2018/10/12/bstdlloriginalbst.png)
>
>  
>
> 我们希望将这个二叉搜索树转化为双向循环链表。链表中的每个节点都有一个前驱和后继指针。对于双向循环链表，第一个节点的前驱是最后一个节点，最后一个节点的后继是第一个节点。
>
> 下图展示了上面的二叉搜索树转化成的链表。“head” 表示指向链表中有最小元素的节点。
>
>  
>
> ![img](https://assets.leetcode.com/uploads/2018/10/12/bstdllreturndll.png)
>
>  
>
> 特别地，我们希望可以就地完成转换操作。当转化完成以后，树中节点的左指针需要指向前驱，树中节点的右指针需要指向后继。还需要返回链表中的第一个节点的指针。
>
>  



```java
	//time : O(n)
    //space : O(n)
    Node pre,head;
    //1.树的有序遍历为中序遍历。
    //2.递归左子树后 将 节点的pre 和 right left关联
    //3.dfs完毕后 将首位节点相连接。
    public Node treeToDoublyList(Node root) {
        if(root == null)    
            return null;
        dfs(root);
        //将首位指针相连接
        head.left = pre;
        pre.right = head;
        return head;
    }

    void dfs(Node cur){
        //1.终止条件
        if(cur == null)
            return;
        //2.递归左子树
        dfs(cur.left);
        //3. pre节点 和 cur的 next 和 pre 相连接。 
        // 等价于 node.right为next节点 node.left为pre节点
        // 如果是pre==null 说明第一次遍历  head = cur
        if(pre != null) 
            pre.right = cur;
        else    
            head = cur;
        cur.left = pre;
        pre = cur;
        // 递归右子树
        dfs(cur.right);
    }
```

## 37. 序列化二叉树 ⭐

> #### [剑指 Offer 37. 序列化二叉树](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/)
>
> 难度困难44
>
> 请实现两个函数，分别用来序列化和反序列化二叉树。
>
> **示例:** 
>
> ```
> 你可以将以下二叉树：
> 
>     1
>    / \
>   2   3
>      / \
>     4   5
> 
> 序列化为 "[1,2,3,null,null,4,5]"
> ```

```java
	/*
    * 思路：
         1、序列化  创建一个builder 用 , 进行分隔 如果是null 则用 x 进行标记。 
            Tree ->  字符串  
         2、反序列化  使用双端队列进行添加搜索元素，x 说明是一个Null 跳过，否则的话 就直接				递归
            查找递归root.left  和  root.right 
    */
    //构建字符串
    private static final String spliter = ",";
    private static final String NN = "x";

    // Encodes a tree to a single string.
    // 构建二叉树序列化-》BST>String
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        buildString(root,sb);
        return sb.toString();
    }

    private void buildString(TreeNode root,StringBuilder sb){
        //root == null
        if(root == null){ // x,
            sb.append(NN).append(spliter);
        }else{
            sb.append(root.val).append(spliter);
            buildString(root.left,sb);
            buildString(root.right,sb);
        }
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        Deque<String> nodes = new LinkedList<>();
        nodes.addAll(Arrays.asList(data.split(spliter)));
        return buildTree(nodes);
    }

    public TreeNode buildTree(Deque<String> nodes){
        String val = nodes.remove();
        if(val.equals(NN)) return null;
        else{
            TreeNode node = new TreeNode(Integer.valueOf(val));
            node.left = buildTree(nodes);
            node.right = buildTree(nodes);
            return node;
        }
    }
```



## 38.字符串的排列 ★

> #### [面试题38. 字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)
>
> 难度中等38
>
> 输入一个字符串，打印出该字符串中字符的所有排列。
>
>  
>
> 你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。
>
>  
>
> **示例:**
>
> ```
> 输入：s = "abc"
> 输出：["abc","acb","bac","bca","cab","cba"]
> ```

### 回溯

```java
	//dfs  time ：O(N!)  space  ： O(N^2)
    List<String> ret = new LinkedList<>();
    char [] ch;
    public String[] permutation(String s) {
        if(s == null){
            return new String[0];
        }
        ch = s.toCharArray();
        dfs(0);
        return ret.toArray(new String[ret.size()]);
    }

    private void dfs(int x){
        //终止条件
        if(x == ch.length-1){
            ret.add(String.valueOf(ch));
            return;
        }
        HashSet<Character> hashSet = new HashSet<>();
        
        for(int i=x;i<ch.length;i++){
            //去重 -》剪枝操作
            if(hashSet.contains(ch[i]))  continue;
            hashSet.add(ch[i]);
            //交换
            swap(i,x);
            dfs(x+1);
            //撤回交换
            swap(x,i);
        }
    }

    private void swap(int x,int i){
        char tmp = ch[x];
        ch[x] = ch[i];
        ch[i] = tmp;
    }
```



## 39.数组中出现次数超过一半的数字 ★

> #### [面试题39. 数组中出现次数超过一半的数字](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)
>
> 难度简单19
>
> 数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。
>
>  
>
> 你可以假设数组是非空的，并且给定的数组总是存在多数元素。
>
>  
>
> **示例 1:**
>
> ```
> 输入: [1, 2, 3, 2, 2, 2, 5, 4, 2]
> 输出: 2
> ```

### 1.哈希表

```java
	//哈希表
    //1.将数组中元素作为key value为出现的次数
    //2.遍历哈希表中所有key 找出出现最大的次数 
    // time ：O(n) 遍历数组n次
    // space : 将元素存储到哈希表中所占用的空间
    public int majorityElement(int[] nums) {
        if(nums == null || nums.length == 0){
            return -1;
        }      
        Map<Integer,Integer> results = new HashMap<>();
        for(int i=0;i<nums.length;i++){
            if(!results.containsKey(nums[i]))
                results.put(nums[i],1);
            else
                results.put(nums[i],results.get(nums[i])+1);
        }

        Map.Entry<Integer, Integer> majorityEntry = null;
        for (Map.Entry<Integer, Integer> entry : results.entrySet()) {
            if (majorityEntry == null || entry.getValue() > majorityEntry.getValue()) {
                majorityEntry = entry;
            }
        }

        return majorityEntry.getKey();
    }
```

### 2.排序

```java
// time : (OlogN) 数组排序需要的时间
    // space ： (logN) 数组排序需要的外部存储空间 语言自身的排序空间是OlogN
    // ps : 当数组中有大于一半以上的数据时，如果将数组进行排序，那么中间下标位置一定是众数的下标。
    // 无论是偶数 还是 奇数。
    // 众数 : 在一组数据中出现次数最多的数字。
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length/2];
    }
```

### 3.Boyer-Moore 投票算法

```java
// Boyer-Moore 投票算法
    // 核心思想 
    // 如果候选人不是maj 则 maj,会和其他非候选人一起反对 会反对候选人,所以候选人一定会下台(maj==0时发生换届选举)
    // 如果候选人是maj , 则maj 会支持自己，其他候选人会反对，同样因为maj 票数超过一半，所以maj 一定会成功当选
    // time : O(n) 一次loop array
    // space : O(1) 
    public int majorityElement(int[] nums) {
        if(nums == null || nums.length == 0) return -1;
        int count = 0;
        Integer candidate = null;

        for(int num : nums){
            if(count == 0){
                candidate = num;//更换候选人
            }
            //count += ((num == candidate) ? 1 : -1);
             count += (num == candidate) ? 1 : -1;
        }
        return candidate;
    }
```

## 40.最小的K个数 ★

> #### [面试题40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)
>
> 难度简单76
>
> 输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。
>
>  
>
> **示例 1：**
>
> ```
> 输入：arr = [3,2,1], k = 2
> 输出：[1,2] 或者 [2,1]
>
> ```
>
> **示例 2：**
>
> ```
> 输入：arr = [0,1,2,1], k = 1
> 输出：[0]
> ```

Top(k) 问题的解法 一般可以用堆 或者  类似快排的思想。

### 1.堆

```java
// time : O(k)
// space : O(nlogk)
//使用一个堆 固定大小 将数组中所有的元素 全部添加到堆中  根据一定的条件 才可以添加 删除
// 最后剩余的就是要的数据
public int[] getLeastNumbers(int[] arr, int k) {
    if (k == 0) {
        return new int[0];
    }
    // 使用一个最大堆（大顶堆）
    // Java 的 PriorityQueue 默认是小顶堆，添加 comparator 参数使其变成最大堆
    Queue<Integer> heap = new PriorityQueue<>(k, (i1, i2) -> Integer.compare(i2, i1));

    for (int e : arr) {
        // 当前数字小于堆顶元素才会入堆
        if (heap.isEmpty() || heap.size() < k || e < heap.peek()) {
            heap.offer(e);
        }
        if (heap.size() > k) {
            heap.poll(); // 删除堆顶最大元素
        }
    }

    // 将堆中的元素存入数组
    int[] res = new int[heap.size()];
    int j = 0;
    for (int e : heap) {
        res[j++] = e;
    }
    return res;
}
```

## 41.数据流中的中位数 ★

> #### [面试题41. 数据流中的中位数](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)
>
> 难度困难26
>
> 如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。
>
> 例如，
>
> [2,3,4] 的中位数是 3
>
> [2,3] 的中位数是 (2 + 3) / 2 = 2.5
>
> 设计一个支持以下两种操作的数据结构：
>
> - void addNum(int num) - 从数据流中添加一个整数到数据结构中。
> - double findMedian() - 返回目前所有元素的中位数。
>
> **示例 1：**
>
> ```
> 输入：
> ["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"]
> [[],[1],[2],[],[3],[]]
> 输出：[null,null,null,1.50000,null,2.00000]
> ```

```java
	Queue<Integer> A,B;

    /** initialize your data structure here. */
    public MedianFinder() {
        A = new PriorityQueue<>();//小顶堆
        B = new PriorityQueue<>((x,y)->(y-x));//大顶堆
    }
    //保证 中间数 一个子啊大顶堆 一个在小顶堆
    public void addNum(int num) {
        if(A.size()!=B.size()){
            A.add(num);
            B.add(A.poll());
        }else{
            B.add(num);
            A.add(B.poll());
        }
    }
    
    public double findMedian() {
        return A.size() != B.size() ? A.peek():(A.peek()+B.peek())/2.0; 
    }
```

## 42.连续子数组的最大和 ★

> #### [面试题42. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)
>
> 难度简单42
>
> 输入一个整型数组，数组里有正数也有负数。数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。
>
> 要求时间复杂度为O(n)。
>
>  
>
> **示例1:**
>
> ```
> 输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
> 输出: 6
> 解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
> ```
>
>  
>
> **提示：**
>
> - `1 <= arr.length <= 10^5`
> - `-100 <= arr[i] <= 100`
>
> 注意：本题与主站 53 题相同：<https://leetcode-cn.com/problems/maximum-subarray/>



```java
public int maxSubArray(int[] nums) {
        if(nums.length == 0 || nums == null){
            return -1;
        }
        int greatestSum = Integer.MIN_VALUE;
        int sum = 0;
        for(int num: nums){
          //如果负数 从新开始 不断累加 求出最大值。
            sum = sum< 0 ? num : sum + num;
            greatestSum = Math.max(greatestSum,sum);
        }
        return greatestSum;
}
```

## 43.1～n整数中1出现的次数

> #### [剑指 Offer 43. 1～n整数中1出现的次数](https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/)
>
> 难度中等70
>
> 输入一个整数 `n` ，求1～n这n个整数的十进制表示中1出现的次数。
>
> 例如，输入12，1～12这些整数中包含1 的数字有1、10、11和12，1一共出现了5次。
>
>  
>
> **示例 1：**
>
> ```
> 输入：n = 12
> 输出：5
> ```
>
> **示例 2：**
>
> ```
> 输入：n = 13
> 输出：6
> ```

```java
	public int countDigitOne(int n) {
        int res = 0;
        for(long m = 1 ; m<=n ;m*=10){
            long a = n/m, b = n % m;
            res+=(a+8)/10*m+(a%10 == 1 ? b+1:0);
        }
        return res;
    }
```



## 48.最长不含重复字符的子字符串

> #### [面试题48. 最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)
>
> 难度中等31
>
> 请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。
>
>  
>
> **示例 1:**
>
> ```
> 输入: "abcabcbb"
> 输出: 3 
> 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
>
> ```
>
> **示例 2:**
>
> ```
> 输入: "bbbbb"
> 输出: 1
> 解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
>
> ```
>
> **示例 3:**
>
> ```
> 输入: "pwwkew"
> 输出: 3
> 解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
>      请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
> ```

## 47.礼物的最大价值  ⭐⭐

> #### [面试题47. 礼物的最大价值](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/)
>
> 难度中等34
>
> 在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？
>
>  
>
> **示例 1:**
>
> ```
> 输入: 
> [
>   [1,3,1],
>   [1,5,1],
>   [4,2,1]
> ]
> 输出: 12
> 解释: 路径 1→3→5→2→1 可以拿到最多价值的礼物
> ```
>
>  

```java
	//采用dp 如果是递归的话 会出现很多重复性计算，复杂度比较高 
    //而dp每次选取最优的解，time : O(n*m) 
    public int maxValue(int[][] grid) {
        if(grid.length == 0 || grid == null || grid[0].length == 0){
            return 0;
        }
        int n = grid[0].length;
        int [] dp = new int [n];
        for(int [] g : grid){
            dp[0]+=g[0];
            for(int i=1;i<n;i++){
                dp[i] = Math.max(dp[i],dp[i-1])+g[i];
            }
        }
        return dp[n-1];
    }
```

## 49.丑数 ★

> #### [面试题49. 丑数](https://leetcode-cn.com/problems/chou-shu-lcof/)
>
> 难度中等26
>
> 我们把只包含因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。
>
>  
>
> **示例:**
>
> ```
> 输入: n = 10
> 输出: 12
> 解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
> ```
>
> **说明: ** 
>
> 1. `1` 是丑数。
> 2. `n` **不超过**1690。



```java
	//dp问题  
    //思路 我们知道丑数是可以被2 or 3 or 5 连续整除的。如果==1 就是丑数
    // 初始化 dp[0] = 1;   1*2=2   1*3=3  1*5 =5 min = 2
    // dp[1] = 2  a=1;
    //  2*2 = 4     1*3 = 3     1*5 = 5     min = 3
    // dp[2] = 3  b=3;
    //如此反复进行计算  dp方程 ：  dp[Math.min(n2,Math.min(n3,n5))] 
    //time :O(n)
    //space : O(n)
    public int nthUglyNumber(int n) {
        if(n<6) return n;
        int [] dp = new int [n];
        dp[0] = 1;
        int a = 0, b = 0, c = 0;
        for(int i=1;i<n;i++){
            int n2 = dp[a] * 2,n3 = dp[b] * 3, n5 = dp[c] * 5;
            dp[i] = Math.min(n2,Math.min(n3,n5));
            if(dp[i] == n2) a++;
            if(dp[i] == n3) b++;
            if(dp[i] == n5) c++;
        }
        return dp[n-1];
    }
```



## 50.第一个出现一次的字符  ⭐⭐

> #### [面试题50. 第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)
>
> 难度简单14
>
> 在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。
>
> **示例:**
>
> ```
> s = "abaccdeff"
> 返回 "b"
>
> s = "" 
> 返回 " "
> ```

### 1.哈希表

```java
	/*
    *1.哈希表
     a.使用hashmap来存储 key为字符，value为true or  false 
       1）true -> 该字符为一个
       2) false -> 该字符不为一个
     b.loop map 找到第一个数量为1的字符 return
     c.直接 return ''; 
     time : O(n)
     space : O(n)
    */
    public char firstUniqChar(String s) {
        Map<Character,Boolean> dataMap = new HashMap<>();
        char [] chs = s.toCharArray();
        for(char ch: chs){
            dataMap.put(ch,!dataMap.containsKey(ch));
        }

        for(char ch : chs){
            if(dataMap.get(ch))
                return ch;
        }
        return ' ';
    }
```

### 2.有序hash表

```java
/*
    * 2.有序hash表
      思路:hash表有去重的特点，当数据比较大的时候 我们直接从hash中查找第一个value为true的值就可以了。
      time : O(n) 而前者为O(2n) 
    */
    public char firstUniqChar(String s) {
        Map<Character, Boolean> dic = new LinkedHashMap<>();
        char[] sc = s.toCharArray();
        for(char c : sc)
            dic.put(c, !dic.containsKey(c));
        for(Map.Entry<Character, Boolean> d : dic.entrySet()){
           if(d.getValue()) return d.getKey();
        }
        return ' ';
    }
```

### 3.数组hash表

```java
	public char firstUniqChar(String s) {
        if(s==null||"".equals(s)){
            return ' ';
        }
        int counts[] = new int[256];
        for(int i = 0;i<s.length();i++){           
            counts[s.charAt(i)]++;         
        }
        
        for(int i = 0;i<s.length();i++){
            if(counts[s.charAt(i)]==1){
                return s.charAt(i);
            }
        }
        return ' ';
    }
```



### 51.数组中的逆序对

> #### [面试题51. 数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)
>
> 难度困难164
>
> 在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。
>
>  
>
> **示例 1:**
>
> ```
> 输入: [7,5,6,4]
> 输出: 5
> ```



## 52.两个链表的第一个公共节点 ⭐⭐

> #### [面试题52. 两个链表的第一个公共节点](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)
>
> 难度简单37
>
> 输入两个链表，找出它们的第一个公共节点。
>
> 如下面的两个链表**：**
>
> [![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)
>
> 在节点 c1 开始相交。
>
>  
>
> **示例 1：**
>
> [![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_1.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)
>
> ```
> 输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
> 输出：Reference of the node with value = 8
> 输入解释：相交节点的值为 8 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
>
> ```
>
>  
>
> **示例 2：**
>
> [![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_2.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png)
>
> ```
> 输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
> 输出：Reference of the node with value = 2
> 输入解释：相交节点的值为 2 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
>
> ```
>
>  
>
> **示例 3：**
>
> [![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_3.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)
>
> ```
> 输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
> 输出：null
> 输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
> 解释：这两个链表不相交，因此返回 null。
>
> ```
>
>  
>
> **注意：**
>
> - 如果两个链表没有交点，返回 `null`.
> - 在返回结果后，两个链表仍须保持原有的结构。
> - 可假定整个链表结构中没有循环。
> - 程序尽量满足 O(*n*) 时间复杂度，且仅用 O(*1*) 内存。
> - 本题与主站 160 题相同：<https://leetcode-cn.com/problems/intersection-of-two-linked-lists/>

```java
/*
    * 1.双指针--链表一般多考虑双指针解法
      思路: A和B先走 谁先走完 走对方的路，直到相遇
      如果没有交叉点，当A为null B也为null 返回null 符合
      time : O(m+n)
      space : O(1)
    */
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode listA = headA,listB = headB;
        while(listA != listB){
            listA = listA == null ? headB : listA.next;
            listB = listB == null ? headA : listB.next;
        }
        return listA;
    }
```



## 53.在排序数组中查找数字 I★

> #### [面试题53 - I. 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)
>
> 难度简单23
>
> 统计一个数字在排序数组中出现的次数。
>
>  
>
> **示例 1:**
>
> ```
> 输入: nums = [5,7,7,8,8,10], target = 8
> 输出: 2
> ```
>
> **示例 2:**
>
> ```
> 输入: nums = [5,7,7,8,8,10], target = 6
> 输出: 0
> ```

### 1.二分查找

```java
	//search问题 先O(n)>二分
    //思路 使用二分查找右边界 和 左边界 right-left-1
    //time ： O(logn)
    //space : O(1)
  public int search(int[] nums, int target) {
        return helper(nums,target)-helper(nums,target-1);
    }

    public int helper(int [] nums,int tar){
        int i = 0, j = nums.length-1;
        while(i<=j){
            int m = i+(j-i)/2;
            if(nums[m]<=tar) i = m+1;
            else j = m-1;
        }
        return i;
    }
```

## 53 - II. 0～n-1中缺失的数字★

> #### [面试题53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)
>
> 难度简单16
>
> 一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。
>
>  
>
> **示例 1:**
>
> ```
> 输入: [0,1,3]
> 输出: 2
>
> ```
>
> **示例 2:**
>
> ```
> 输入: [0,1,2,3,4,5,6,7,9]
> 输出: 8
> ```



```java
	//有序数组 考虑使用二分查找
    // 求出midNum
    // time : O(logn)
    // space : O(1) 
    public int missingNumber(int[] nums) {
        int i = 0, j = nums.length - 1;
        while(i<=j){
            int m = (i+j)/2;
            if(nums[m]==m) i = m+1; //nums[m] == m  从0开始 所以下标值对应m
            else
                j = m -1 ;
       }
       return i;
    }
```

## 54.二叉树的第K大节点 ⭐⭐

> #### [面试题54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)
>
> 难度简单21
>
> 给定一棵二叉搜索树，请找出其中第k大的节点。
>
>  
>
> **示例 1:**
>
> ```
> 输入: root = [3,1,4,null,2], k = 1
>    3
>   / \
>  1   4
>   \
>    2
> 输出: 4
> ```
>
> **示例 2:**
>
> ```
> 输入: root = [5,3,6,2,4,null,null,1], k = 3
>        5
>       / \
>      3   6
>     / \
>    2   4
>   /
>  1
> 输出: 4
> ```



```java
//中序遍历为顺序的 右根左就位 从高到低 k记录的是第几大元素。当k==0就可以直接返回。
    int res,k;
    public int kthLargest(TreeNode root, int k) {
        if(root == null)    return -1;
        this.k = k;
        dfs(root);
        return res;
    }

    private void dfs(TreeNode root){
        if(root == null)    return;
        dfs(root.right);
        if(k<0) return;
        if(--k == 0)    res = root.val;
        dfs(root.left);
    }
```

##  55 - I. 二叉树的深度 ⭐

> #### [剑指 Offer 55 - I. 二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)
>
> 难度简单27
>
> 输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。
>
> 例如：
>
> 给定二叉树 `[3,9,20,null,null,15,7]`，
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
> ```

### dfs

```java
public int maxDepth(TreeNode root) {
        return root == null ? 0 : Math.max(maxDepth(root.left),maxDepth(root.right))+1;
    }
```



## 55.-II 平衡二叉树 ⭐

> #### [面试题55 - II. 平衡二叉树](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)
>
> 难度简单19
>
> 输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。
>
>  
>
> **示例 1:**
>
> 给定二叉树 `[3,9,20,null,null,15,7]`
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
> ```
>
> 返回 `true` 。
> **示例 2:**
>
> 给定二叉树 `[1,2,2,3,3,null,null,4,4]`
>
> ```
>        1
>       / \
>      2   2
>     / \
>    3   3
>   / \
>  4   4
>
> ```
>
> 返回 `false` 。

```java
    private boolean isBalanced = true;//默认为true

    public boolean isBalanced(TreeNode root) {
        dfs(root);
        return isBalanced;
    }

    private int dfs(TreeNode root){
        if(root == null || !isBalanced){
            return 0;
        }
        int left = dfs(root.left); //计算左子节点长度
        int right = dfs(root.right);//计算右子节点长度
        if(Math.abs(left-right)>1){//如果左右子节点长度差大于1 则为false
            isBalanced = false;
        }
        return 1+Math.max(left,right);
    }
```

## 57.和为s的两个数字 ★

> #### [面试题57. 和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)
>
> 难度简单18
>
> 输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。
>
> 
>
> **示例 1：**
>
> ```
> 输入：nums = [2,7,11,15], target = 9
> 输出：[2,7] 或者 [7,2]
>
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [10,26,30,31,47,60], target = 40
> 输出：[10,30] 或者 [30,10]
> ```

### 二分查找

```java
	//二分查找 binary search
    // time : O(n)
    // space : O(1)
    public int[] twoSum(int[] nums, int target) {
        int i = 0, j = nums.length-1;
        while(i<j){
            if(nums[i]+nums[j]<target) i++;
            else if(nums[i]+nums[j]>target) j--;
            else if(nums[i]+nums[j] == target){
                return new int[]{nums[i],nums[j]};
            }
        } 
        return new int [0];
    }
```

## 57 - II. 和为s的连续正数序列

> #### [面试题57 - II. 和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)
>
> 难度简单98
>
> 输入一个正整数 `target` ，输出所有和为 `target` 的连续正整数序列（至少含有两个数）。
>
> 序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。
>
>  
>
> **示例 1：**
>
> ```
> 输入：target = 9
> 输出：[[2,3,4],[4,5]]
>
> ```
>
> **示例 2：**
>
> ```
> 输入：target = 15
> 输出：[[1,2,3,4,5],[4,5,6],[7,8]]
> ```

### 双指针

```java
	//双指针
    //这类属于窗口问题，i j分别指向前后 当sum小于target j++ sum大于target i++ 不断缩减区		间。 time : O(n) space : O(n)
    public int[][] findContinuousSequence(int target) {
        if(target<0)    return new int [0][0];
        int i = 1;//慢指针
        int j = 1;//快指针
        int sum = 0;
        List<int []> result = new ArrayList<>();
        while(i<=target/2){
            if(sum<target){//如果小于 j++
                sum+=j;
                j++;
            }else if(sum > target){//如果大于i++ 
                sum-=i;
                i++;
            }else{
                int [] arr = new int [j-i];
                for(int k=i;k<j;k++){
                    arr[k-i] = k;
                }
                result.add(arr);
                sum-=i;
                i++;
            }
        } 
        return result.toArray(new int[result.size()][]);
    }
```



## [58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/) ★

> #### [面试题58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)
>
> 难度简单13
>
> 输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。
>
>  
>
> **示例 1：**
>
> ```
> 输入: "the sky is blue"
> 输出: "blue is sky the"
>
> ```
>
> **示例 2：**
>
> ```
> 输入: "  hello world!  "
> 输出: "world! hello"
> 解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
>
> ```
>
> **示例 3：**
>
> ```
> 输入: "a good   example"
> 输出: "example good a"
> 解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
> ```

```java
	//time : O(n)  space : O(m) 
    //按照 " " 划分数组 逆序拼接到strs 如果包含 "" 跳过。
    public String reverseWords(String s) {
      String [] str = s.split(" ");
      String strs = "";
       //注意split后多出的是空的字符串数组元素即("")而非(" ")
      for(int i=str.length-1;i>=0;i--){
          if(!str[i].equals("")){
              strs+=str[i]+" ";
          }
      }
      return strs.trim();
    }
```

## 58 - II. 左旋转字符串★

> #### [面试题58 - II. 左旋转字符串](https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)
>
> 难度简单24
>
> 字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。
>
>  
>
> **示例 1：**
>
> ```
> 输入: s = "abcdefg", k = 2
> 输出: "cdefgab"
>
> ```
>
> **示例 2：**
>
> ```
> 输入: s = "lrloseumgh", k = 6
> 输出: "umghlrlose"面试题58 - II. 左旋转字符串
> 字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。
>
>  
>
> 示例 1：
>
> 输入: s = "abcdefg", k = 2
> 输出: "cdefgab"
> 示例 2：
>
> 输入: s = "lrloseumgh", k = 6
> 输出: "umghlrlose"
> ```



```java
	// time : O(n)  space : O(1)
    // 思路 abcd n = 2  ab >ba cd > dc  badc  cdab
    public String reverseLeftWords(String s, int n) {
        if(s.length() < n)    return s;

        char [] ch = s.toCharArray();

        //转换前半部分
        reverse(ch,0,n-1);
        //转换后半部分
        reverse(ch,n,s.length()-1);
        //转换整体
        reverse(ch,0,s.length()-1);
        return new String(ch);
    }

    private void reverse(char []ch,int i,int j){
        while(i<j){
            swap(ch,i++,j--);
        }
    }

    //交换
    private void swap(char [] ch,int i,int j){
        char tmp = ch[i];
        ch[i] = ch[j];
        ch[j] = tmp;
    }
```





## 59.-1 滑动窗口的最大值 ⭐⭐

> #### [面试题59 - I. 滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)
>
> 难度简单41
>
> 给定一个数组 `nums` 和滑动窗口的大小 `k`，请找出所有滑动窗口里的最大值。
>
> **示例:**
>
> ```
> 输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
> 输出: [3,3,5,5,6,7] 
> 解释: 
>
>   滑动窗口的位置                最大值
> ---------------               -----
> [1  3  -1] -3  5  3  6  7       3
>  1 [3  -1  -3] 5  3  6  7       3
>  1  3 [-1  -3  5] 3  6  7       5
>  1  3  -1 [-3  5  3] 6  7       5
>  1  3  -1  -3 [5  3  6] 7       6
>  1  3  -1  -3  5 [3  6  7]      7
> ```
>
>  
>
> **提示：**
>
> 你可以假设 *k *总是有效的，在输入数组不为空的情况下，1 ≤ k ≤ 输入数组的大小。

### 1.穷举法

```java
	//1.穷求法
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums.length == 0 || nums == null || k == 0) 
            return new int[0];
        //移动的次数为 length-k+1  
        int [] array = new int [nums.length-k+1];
        for(int i=0;i<array.length;i++){
            int maxNum = Integer.MIN_VALUE;
            for(int j = i;j<i+k;j++){
                maxNum = Math.max(maxNum,nums[j]);
            }
            array[i] = maxNum;
        }
        return array;
    }
```

### 2.双端队列

```java
	private ArrayDeque<Integer> arrayDeque  = new ArrayDeque<>();

    private int [] nums;

    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums == null || nums.length * k == 0){
            return new int [0];
        }

        this.nums = nums;
        int n = nums.length;
        int [] arr = new int [n-k+1];
        int maxIndex = 0;
        for(int i=0;i<k;i++){
            cleanDeq(i,k);
            arrayDeque.addLast(i);
            maxIndex = nums[maxIndex] > nums[i] ? maxIndex : i;
        }

        arr[0] = nums[maxIndex];
        for(int i=k;i<n;i++){
            cleanDeq(i,k);
            arrayDeque.addLast(i);
            arr[i-k+1] = nums[arrayDeque.getFirst()];
        }
        return arr;
    }

    private void cleanDeq(int i,int k){
        if(!arrayDeque.isEmpty()&&arrayDeque.getFirst() == i-k){
            arrayDeque.removeFirst();
        }
        while(!arrayDeque.isEmpty()&&nums[i]>nums[arrayDeque.getLast()]){
            arrayDeque.removeLast();
        }
    }
```

## 59 - ||队列的最大值 ★

> #### [面试题59 - II. 队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)
>
> 难度中等92
>
> 请定义一个队列并实现函数 `max_value` 得到队列里的最大值，要求函数`max_value`、`push_back` 和 `pop_front` 的**均摊**时间复杂度都是O(1)。
>
> 若队列为空，`pop_front` 和 `max_value` 需要返回 -1
>
> **示例 1：**
>
> ```
> 输入: 
> ["MaxQueue","push_back","push_back","max_value","pop_front","max_value"]
> [[],[1],[2],[],[],[]]
> 输出: [null,null,null,2,1,2]
>
> ```
>
> **示例 2：**
>
> ```
> 输入: 
> ["MaxQueue","pop_front","max_value"]
> [[],[],[]]
> 输出: [null,-1,-1]
> ```

### 双端队列

```java
    Deque<Integer> que;
    Deque<Integer> deq;

    public MaxQueue() {
        que = new LinkedList<>();//add and dele 
        deq = new LinkedList<>();//find max value 
    }
    
    public int max_value() {
        return que.size()>0 ? deq.peek() : -1;
    }
    
    public void push_back(int value) {
        que.offer(value);
        while(deq.size()>0 && deq.peekLast()<value){
            deq.pollLast();
        }
        deq.offer(value);
    }
    
    public int pop_front() {
        int tmp = que.size()>0? que.poll() : -1;
        //deq size > 0 and tmp 包含 maxValue 返回 deq的最大值
        if(deq.size()>0 && tmp == deq.peek()){
            deq.poll();
        }
        return tmp;
    }
```

## 60.[n个骰子的点数](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/)

> #### [面试题60. n个骰子的点数](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/)
>
> 难度简单46
>
> 把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率。
>
>  
>
> 你需要用一个浮点数数组返回答案，其中第 i 个元素代表这 n 个骰子所能掷出的点数集合中第 i 小的那个的概率。
>
>  
>
> **示例 1:**
>
> ```
> 输入: 1
> 输出: [0.16667,0.16667,0.16667,0.16667,0.16667,0.16667]
>
> ```
>
> **示例 2:**
>
> ```
> 输入: 2
> 输出: [0.02778,0.05556,0.08333,0.11111,0.13889,0.16667,0.13889,0.
> ```

## 61.扑克牌中的顺子

> #### [面试题61. 扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)
>
> 难度简单24
>
> 从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。
>
>  
>
> **示例 1:**
>
> ```
> 输入: [1,2,3,4,5]
> 输出: True
> ```
>
>  
>
> **示例 2:**
>
> ```
> 输入: [0,0,1,2,5]
> 输出: True
> ```

```java
 /*
    a.最大元素-nums[joker] 小于 5 不可以匹配，或者当前和后一个元素相等 不能成顺子
    */
    public boolean isStraight(int[] nums) {
        int joker = 0;
        Arrays.sort(nums);
        for(int i=0;i<4;i++){
            if(nums[i]==0){
                joker++;
            }else if(nums[i]==nums[i+1]){
                return false;
            }
        }
        return nums[4]-nums[joker] < 5;
    }
```

## [62. 圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)  ★

> #### [面试题62. 圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)
>
> 难度简单139
>
> 0,1,,n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字。求出这个圆圈里剩下的最后一个数字。
>
> 例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。
>
>  
>
> **示例 1：**
>
> ```
> 输入: n = 5, m = 3
> 输出: 3
> ```
>
> **示例 2：**
>
> ```
> 输入: n = 10, m = 17
> 输出: 2
> ```

```java
public int lastRemaining(int n, int m) {
       int res=0;
        for(int i=2;i<=n;i++)
            res=(res+m)%i;
        return res;
    }
```



## 64.求1+2+…+n ★

> #### [面试题64. 求1+2+…+n](https://leetcode-cn.com/problems/qiu-12n-lcof/)
>
> 难度中等66
>
> 求 `1+2+...+n` ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。
>
>  
>
> **示例 1：**
>
> ```
> 输入: n = 3
> 输出: 6
>
> ```
>
> **示例 2：**
>
> ```
> 输入: n = 9
> 输出: 45
> ```

```java
public int sumNums(int n) {
        int sum = n;
        boolean b = (n > 0) && ((sum += sumNums(n - 1)) > 0);
        return sum;
    }
```

## 63.股票的最大利润  ⭐⭐

> #### [面试题63. 股票的最大利润](https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/)
>
> 难度中等21
>
> 假设把某股票的价格按照时间先后顺序存储在数组中，请问买卖该股票一次可能获得的最大利润是多少？
>
>  
>
> **示例 1:**
>
> ```
> 输入: [7,1,5,3,6,4]
> 输出: 5
> 解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
>      注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
> ```

```java
public int maxProfit(int[] prices) {
        int n = prices.length;
        if(n<2) return 0;
        int dp_i_0 = 0, dp_i_1 = Integer.MIN_VALUE;
        for(int i=0;i<n;i++){
            dp_i_0 = Math.max(dp_i_0,dp_i_1+prices[i]);
            dp_i_1 = Math.max(dp_i_1,-prices[i]);
        }
        return dp_i_0;
    }
```







## 65.不用加减乘除做加法

> #### [面试题65. 不用加减乘除做加法](https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/)
>
> 难度简单26
>
> 写一个函数，求两个整数之和，要求在函数体内不得使用 “+”、“-”、“*”、“/” 四则运算符号。
>
>  
>
> **示例:**
>
> ```
> 输入: a = 1, b = 1
> 输出: 2
> ```



```java
/*
* 可以将元素进行划分 相加 和 进位操作。
^ 相加 &进位
*/
public int add(int a, int b) {
        int tmp = 0;
        while(a!=0){
            tmp = a ^ b; // 011 ^ 1000  1011
            a = (a & b) << 1; // 1011   10110
            b = tmp;
        }
     return b;
 }
```



## 66.构建乘积数组

> #### [面试题66. 构建乘积数组](https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof/)
>
> 难度简单16
>
> 给定一个数组 `A[0,1,…,n-1]`，请构建一个数组 `B[0,1,…,n-1]`，其中 `B` 中的元素 `B[i]=A[0]×A[1]×…×A[i-1]×A[i+1]×…×A[n-1]`。不能使用除法。
>
>  
>
> **示例:**
>
> ```
> 输入: [1,2,3,4,5]
> 输出: [120,60,40,30,24]
> ```
>
>  

```java
public int[] constructArr(int[] a) {
        if(a.length == 0)   return new int[0];
        int length = a.length;
        int [] b = new int [length];
        b[0] = 1;
        int tmp = 1;
        for(int i=1;i<length;i++){
            b[i] = b[i-1] * a[i-1];
        }

        for(int i=length-2;i>=0;i--){
            tmp *= a[i+1];
            b[i] *= tmp;
        }
        return b;
    }
```

## 67.把字符串转换成整数 ★

> #### [面试题67. 把字符串转换成整数](https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/)
>
> 难度中等9
>
> 写一个函数 StrToInt，实现把字符串转换成整数这个功能。不能使用 atoi 或者其他类似的库函数。
>
>  
>
> 首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。
>
> 当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。
>
> 该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。
>
> 注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。
>
> 在任何情况下，若函数不能进行有效的转换时，请返回 0。
>
> **说明：**
>
> 假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231, 231 − 1]。如果数值超过这个范围，请返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。
>
> **示例 1:**
>
> ```
> 输入: "42"
> 输出: 42
> ```

```java
	public int strToInt(String str) {
        char [] chs = str.trim().toCharArray();
        if(chs.length == 0)   return 0;
        int res = 0,budry = Integer.MAX_VALUE/10;
        int i=1,sign = 1;
        if(chs[0] == '-') sign = -1;
        else if(chs[0] !='+') i = 0;
        for(int j=i;j<chs.length;j++){
            if(chs[j]<'0' || chs[j] > '9') break;
            if(res > budry || res == budry && chs[j] > '7')
                return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
            res = res * 10 +(chs[j] - '0');
        }
        return sign * res;
    }
```



## [68 - I. 二叉搜索树的最近公共祖先  ](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/)⭐⭐

> #### [面试题68 - I. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/)
>
> 难度简单36
>
> 给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。
>
> [百度百科](https://baike.baidu.com/item/最近公共祖先/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”
>
> 例如，给定如下二叉搜索树: root = [6,2,8,0,4,7,9,null,null,3,5]
>
> ![img](e:\pic\binarysearchtree_improved.png)
>
>  
>
> **示例 1:**
>
> ```
> 输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
> 输出: 6 
> 解释: 节点 2 和节点 8 的最近公共祖先是 6。
> ```

```java
	 //1.BST的特点是左子树的值小于root.val 右子树的值大于root.val
    //2.如果p.val < root.val && q.val < root.val 从左子树递归查找
    //3.如果p.val > root.val && q.val > root.val 从右子树递归查找
    //4.否则的话 返回root
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(p.val < root.val && q.val < root.val){
            return lowestCommonAncestor(root.left,p,q);
        }
        if(p.val > root.val && q.val > root.val){
            return lowestCommonAncestor(root.right,p,q);
        }
            return root;
    }
```

## [68 - II. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)  ⭐⭐

> #### [面试题68 - II. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)
>
> 难度简单77
>
> 给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。
>
> [百度百科](https://baike.baidu.com/item/最近公共祖先/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”
>
> 例如，给定如下二叉树: root = [3,5,1,6,2,0,8,null,null,7,4]
>
> ![img](e:\pic\binarytree.png)
>
>  
>
> **示例 1:**
>
> ```
> 输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
> 输出: 3
> 解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
> ```

```java
	//1. root 为null p 或者 q等于root 返回根节点
    //2.递归查找左子节点 左子节点替换成root 
    //3.递归查找右子节点 右子节点替换成root
    //4.a.left and right为null 返回root  
    //  b.left == null or right == null 返回 right or left 
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || root == p || root == q)  return root;
        TreeNode left = lowestCommonAncestor(root.left,p,q);
        TreeNode right = lowestCommonAncestor(root.right,p,q);
        return left == null ? right : right == null ? left : root;
    }
```

