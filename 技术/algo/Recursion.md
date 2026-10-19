# 递归

## 70.爬楼梯  ☆

> #### [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)
>
> 难度简单943
>
> 假设你正在爬楼梯。需要 *n* 阶你才能到达楼顶。
>
> 每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
>
> **注意：**给定 *n* 是一个正整数。
>
> **示例 1：**
>
> ```
> 输入： 2
> 输出： 2
> 解释： 有两种方法可以爬到楼顶。
> 1.  1 阶 + 1 阶
> 2.  2 阶
> ```
>
> **示例 2：**
>
> ```
> 输入： 3
> 输出： 3
> 解释： 有三种方法可以爬到楼顶。
> 1.  1 阶 + 1 阶 + 1 阶
> 2.  1 阶 + 2 阶
> 3.  2 阶 + 1 阶
> ```

### 1.递归

时间复杂度:O(2^n)

空间复杂度:O(n)

```java
	//暴力破解
    public int climbStairs(int n) {
        return climbStairs(0,n);
    }

    // i记录当前阶数  n代表目标阶数
    public int climbStairs(int i,int n){
        if(i>n){
            return 0;
        }
        if(i == n){
            return 1;
        }
        return climbStairs(i+1,n)+climbStairs(i+2,n);
    }
```

### 2.记忆化递归

可以看到第一种解法 会出现重复计算。

时间复杂度：O(n)

空间复杂度:  O(n)

```java
//记忆化递归
    public int climbStairs(int n) {
        int [] memo = new int [n+1];
        return climbStairs(0,n,memo);
    }

    public int climbStairs(int i,int n,int [] memo){
        if(i>n){
            return 0;
        }
        if(i == n){
            return 1;
        }
        if(memo[i]>0){
            return memo[i];
        }
        memo[i] = climbStairs(i+1,n,memo)+climbStairs(i+2,n,memo);
        return memo[i];
    }
```

### 3.动态规划

time : O(n)

space：O(n)

用一个数组存储 n阶需要的步数，自底向上编程，先求出最开始的 一步一步向上求解。而递归虽然直接求解的是n阶的所需要的步数，但是由于不断地递归调用自身，也就先求解出最小的阶数，一步一步向上求解。

```java
//动态规划  ->自底向上编程
    public int climbStairs(int n) {
        if( n ==  1) 
            return 1;
        int [] dp = new int [n+1];
        dp[1] =  1;
        dp[2] =  2;
        for(int i=3;i<=n;i++){
            dp[i] = dp[i-1] + dp[i-2];
        }
        return dp[n];
    }
```

### 4.斐波那契数列

时间复杂度:O(n)

空间复杂度:O(1)

```java
//斐波那契数列
    public int climbStairs(int n) {
        if(n == 1){
            return 1;
        }
        int first = 1;
        int second = 2;
        int third = 0;
        for(int i=3;i<=n;i++){
            third = first + second; // 1 + 2
            first = second; // 2
            second = third; // 3
        }
        return second;
    }
```

## 22.括号生成  ☆

> #### [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)
>
> 难度中等969
>
> 数字 *n* 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的 **括号组合。
>
>  
>
> **示例：**
>
> ```
> 输入：n = 3
> 输出：[
>        "((()))",
>        "(()())",
>        "(())()",
>        "()(())",
>        "()()()"
>      ]
> ```

时间：O(2^n)

思路:如果递归生成括号，会出现很多无效的括号，因此 在递归的过程中过滤掉一些不符合条件的括号。规律就是 生成n 也就是2n个括号 由于都是小括号，因此 左括号 和右括号 应该是相等的。如果left < n 继续 递归  由于一个右括号应该匹配一个左括号 当左括号 > 右括号 递归  当left 和 right == n的时候就停止 返回。

```java
	private List<String> result;//存储结果

    public List<String> generateParenthesis(int n) {
        result = new ArrayList<String>();
        generate("",0,0,n);
        return result;
    }
    //在recursion中过滤掉不符合条件的括号。
    public void generate(String str,int left,int right,int n){
        //终止条件
        if(left == n && right == n){
            result.add(str);
            return;
        }
        if(left < n){
            generate(str+"(",left+1,right,n);
        }
        if(left > right){
            generate(str+")",left,right+1,n);
        }
    }
```

## 98.验证二叉搜索树 ☆

> #### [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)
>
> 难度中等513
>
> 给定一个二叉树，判断其是否是一个有效的二叉搜索树。
>
> 假设一个二叉搜索树具有如下特征：
>
> - 节点的左子树只包含**小于**当前节点的数。
> - 节点的右子树只包含**大于**当前节点的数。
> - 所有左子树和右子树自身必须也是二叉搜索树。
>
> **示例 1:**
>
> ```
> 输入:
>     2
>    / \
>   1   3
> 输出: true
>
> ```
>
> **示例 2:**
>
> ```
> 输入:
>     5
>    / \
>   1   4
>      / \
>     3   6
> 输出: false
> 解释: 输入为: [5,1,4,null,null,3,6]。
>      根节点的值为 5 ，但是其右子节点值为 4 。
> ```

### 1.中序遍历

```java
	//inoder 遍历的结果就是一个有序的对于BST来说，
    //因此 我们借助stack 左一次inorder 记录每一次的当前值和下一个值比较 
    //如果下一个值小于上一个值 说明 不是BST。否则就是
    //因此 先迭代左子节点 在迭代右子节点
    //time : O(n) 每一个节点有且仅被访问一次
    //space : O(n) 借助一个stack结构存储节点。
    public boolean isValidBST(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        double minValue = -Double.MAX_VALUE;

        while(!stack.isEmpty() || root != null){
            while(root!=null){
                stack.push(root);
                root = root.left;//递归左子节点
            }

            root = stack.pop();

            if(root.val <= minValue) return false; //如果小于最小值 非BST
            minValue = root.val;
            root = root.right;//递归右子节点
        }
        return true;
    }
```

### 2.递归

time:o(n)

```java
//递归 
    //主要思路 就是通过递归查找根节点的左子树的最大值是否大于根节点
    //或者根节点的右子树的最小值是否小于根节点。
    public boolean isValidBST(TreeNode root) {
        return isValid(root,null,null);
    }

    public boolean isValid(TreeNode root,Integer min,Integer max){
        //如果根节点为null 返回true
        if(root == null)  return true;
        if(min != null && root.val <= min) return false;
        if(max != null && root.val >= max) return false;

        return isValid(root.left,min,root.val) && isValid(root.right,root.val,max);
    }
```

## 226.翻转二叉树

> #### [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)
>
> 难度简单413
>
> 翻转一棵二叉树。
>
> **示例：**
>
> 输入：
>
> ```
>      4
>    /   \
>   2     7
>  / \   / \
> 1   3 6   9
> ```
>
> 输出：
>
> ```
>      4
>    /   \
>   7     2
>  / \   / \
> 9   6 3   1
> ```

### 1.DFS

time : O(n)

space: O(n) 最好 O(1)  最坏 O(n)退换成链表

```java
//1.递归  DFS
    public TreeNode invertTree(TreeNode root) {
        if(root == null)    return null;
        TreeNode tmp = root.left;  
        root.left = invertTree(root.right);  
        root.right = invertTree(tmp);
        return root;
    }
```

### 2.栈

time：O(n)

space：O(n)

```java
//栈
    public TreeNode invertTree(TreeNode root) {
        if(root == null) return null;
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()){
            TreeNode t = stack.pop();
            if(t.left != null) stack.push(t.left);
            if(t.right != null) stack.push(t.right);
            TreeNode tmp = t.left;
            t.left = t.right;
            t.right = tmp;  
        }
        return root;
    }
```

## 104.二叉树的最大深度  ☆

> #### [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)
>
> 难度简单513
>
> 给定一个二叉树，找出其最大深度。
>
> 二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。
>
> **说明:** 叶子节点是指没有子节点的节点。
>
> **示例：**
> 给定二叉树 `[3,9,20,null,null,15,7]`，
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
> ```
>
> 返回它的最大深度 3 。

```java
	//DFS
    public int maxDepth(TreeNode root) {
        return root == null ? 0 : 			Math.max(maxDepth(root.left),maxDepth(root.right))+1;
    }
```

## 111.二叉树的最小深度 ☆

> #### [111. 二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)
>
> 难度简单244
>
> 给定一个二叉树，找出其最小深度。
>
> 最小深度是从根节点到最近叶子节点的最短路径上的节点数量。
>
> **说明:** 叶子节点是指没有子节点的节点。
>
> **示例:**
>
> 给定二叉树 `[3,9,20,null,null,15,7]`,
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
> ```
>
> 返回它的最小深度  2.



```java
	//分治
    public int minDepth(TreeNode root) {
        if(root == null)  return 0;
        int left = minDepth(root.left);
        int right = minDepth(root.right);
        return (left == 0 || right == 0) ? left+right+1 : Math.min(left,right)+1;
    }
```

## 235.二叉搜索树的最近公共祖先

> #### [235. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)
>
> 难度简单266
>
> 给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。
>
> [百度百科](https://baike.baidu.com/item/%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”
>
> 例如，给定如下二叉搜索树:  root = [6,2,8,0,4,7,9,null,null,3,5]
>
> ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/binarysearchtree_improved.png)
>
>  
>
> **示例 1:**
>
> ```
> 输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
> 输出: 6 
> 解释: 节点 2 和节点 8 的最近公共祖先是 6。
>
> ```
>
> **示例 2:**
>
> ```
> 输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
> 输出: 2
> 解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。
> ```

time:O(n)

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

## 236.二叉树的最近公共祖先

> #### [236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)
>
> 难度中等466
>
> 给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。
>
> [百度百科](https://baike.baidu.com/item/%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”
>
> 例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]
>
> ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/binarytree.png)
>
>  
>
> **示例 1:**
>
> ```
> 输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
> 输出: 3
> 解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
>
> ```
>
> **示例 2:**
>
> ```
> 输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
> 输出: 5
> 解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
> ```

time:O(n)

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

## 297.二叉树的序列化与反序列化

> #### [297. 二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)
>
> 难度困难176
>
> 序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。
>
> 请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。
>
> **示例: **
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
>
> **提示: **这与 LeetCode 目前使用的方式一致，详情请参阅 [LeetCode 序列化二叉树的格式](https://leetcode-cn.com/faq/#binary-tree)。你并非必须采取这种方式，你也可以采用其他的方法解决这个问题。
>
> **说明: **不要使用类的成员 / 全局 / 静态变量来存储状态，你的序列化和反序列化算法应该是无状态的。



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

## 105.从前序与中序遍历序列构造二叉树

> #### [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
>
> 难度中等422
>
> 根据一棵树的前序遍历与中序遍历构造二叉树。
>
> **注意:**
> 你可以假设树中没有重复的元素。
>
> 例如，给出
>
> ```
> 前序遍历 preorder = [3,9,20,15,7]
> 中序遍历 inorder = [9,3,15,20,7]
> ```
>
> 返回如下的二叉树：
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
> ```

递归

```java
	//1.前序遍历的顺序为 根左右。
    //2.map存储中序遍历 key为值 value 为index
    Map<Integer,Integer> result = new HashMap<>();
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for(int i=0;i<inorder.length;i++){
            result.put(inorder[i],i);
        }
        return recur(preorder,0,preorder.length-1,0);
    }

    public TreeNode recur(int [] preorder,int preL,int preR,int inoL){
        if(preL>preR)  return null;
        TreeNode root = new TreeNode(preorder[preL]);
        int index = result.get(root.val);
        int indexSize = index - inoL;
        //重建左子树
        root.left = recur(preorder,preL+1,preL+indexSize,inoL);
        //重建右子树
        root.right = recur(preorder,preL+1+indexSize,preR,inoL+indexSize+1);
        return root;
    }
```

## 77.组合

> #### [77. 组合](https://leetcode-cn.com/problems/combinations/)
>
> 难度中等256
>
> 给定两个整数 *n* 和 *k*，返回 1 ... *n *中所有可能的 *k* 个数的组合。
>
> **示例:**
>
> ```
> 输入: n = 4, k = 2
> 输出:
> [
>   [2,4],
>   [3,4],
>   [2,3],
>   [1,2],
>   [1,3],
>   [1,4],
> ]
> ```

### 回溯法

回溯法是一种选优搜索法，按选优条件向前搜索，已达到目标，但当搜索到某一步时，发现原先选择并不优或达不到目标，就退回一步重新选择，这种走不通就退回在走的技术为回溯法。

```java
public static List<List<Integer>> combine(int n,int k){
        List<List<Integer>> combs = new ArrayList<>();
        combine(combs,new ArrayList<Integer>(),1,n,k);
        return combs;
    }

    private static void combine(List<List<Integer>> combs, ArrayList<Integer> comb, int start, int n, int k) {
        if(k == 0){
            combs.add(new ArrayList<Integer>(comb));
            return;
        }
        for (int i = start; i <= n; i++) {
            comb.add(i);
            combine(combs,comb,i+1,n,k-1);
            comb.remove(comb.size()-1); //执行到这一步 说明 在combs里添加过一次  比如 1,2  需要删除 2   [1]去和下一次配对
            //它的作用就是每成功添加一对的话 删除最后一个。再次配对  [1,2] [1,3] [1,4]  [2,3] [2,4] [3,4]
        }
    }
```

## 46.全排列

> #### [46. 全排列](https://leetcode-cn.com/problems/permutations/)
>
> 难度中等680
>
> 给定一个** 没有重复** 数字的序列，返回其所有可能的全排列。
>
> **示例:**
>
> ```
> 输入: [1,2,3]
> 输出:
> [
>   [1,2,3],
>   [1,3,2],
>   [2,1,3],
>   [2,3,1],
>   [3,1,2],
>   [3,2,1]
> ]
> ```

### 回溯算法: 

```java
回溯算法框架
	for 选择 in 选择列表:
    	# 做选择
    	将该选择从选择列表移除
    	路径.add(选择)
    	backtrack(路径, 选择列表)
    	# 撤销选择
    	路径.remove(选择)
    	将该选择再加入选择列表
    
回溯算法是暴力求解，一般复杂度比较高。而动态规划存在重叠子问题 可以优化。
```

```java
	//回溯算法
    List<List<Integer>> result = new ArrayList<>();

    public List<List<Integer>> permute(int[] nums) {
        if(nums == null || nums.length == 0)    return null;
        permute(nums,new LinkedList<Integer>());
        return result;
    }

    public void permute(int [] nums,LinkedList<Integer> track){
        //1.终止条件
        if(nums.length == track.size()){
            result.add(new LinkedList(track)); //结果集添加
            return;
        }
        for(int i=0;i<nums.length;i++){
            //2.排除不合法的选项 
            if(track.contains(nums[i]))   continue;
            //3.做选择
            track.add(nums[i]);
            //4.进入下层决策树
            permute(nums,track);
            //5.撤销选择
            track.removeLast();
        }
    }
```

## 47.全排列 II

> #### [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/)
>
> 难度中等281
>
> 给定一个可包含重复数字的序列，返回所有不重复的全排列。
>
> **示例:**
>
> ```
> 输入: [1,1,2]
> 输出:
> [
>   [1,1,2],
>   [1,2,1],
>   [2,1,1]
> ]
> ```

此问题和46是相同的思路，使用回溯法 在查找所有解的时候 使用剪枝 取出掉不符合的条件。

代码的实现主要是通过一个boolean数组来判断。如果访问过 continue 如果当前和上一个元素相等  并且上一个元素已经visited 直接continue 。否则就添加。剩下就是回溯法的模板代码。

选择 》 进入下层决策树 》撤销树

```java
List<List<Integer>> result = new ArrayList<>();
    //backtrack
    public List<List<Integer>> permuteUnique(int[] nums) {
        if(nums.length == 0 || nums == null)    return null;
        Arrays.sort(nums);
        findUnique(nums,0,new boolean[nums.length],new LinkedList<Integer>());
        return result;
    }

    private void findUnique(int [] nums,int start,boolean [] visited,LinkedList<Integer>  track){
        //1.终止条件
        if(nums.length ==  track.size()){
            result.add(new LinkedList<>(track));
            return;
        }

        for(int i=0;i<nums.length;i++){
            //如果访问过 直接continue
            if(visited[i]) continue;
            //如果当前节点和前一个节点相等 并且 前一个节点已经被访问过了，直接continue
            if(i>0 && nums[i] == nums[i-1] && visited[i-1])  continue;
            
            //选择
            track.add(nums[i]);
            visited[i] = true;
            //进入下一层决策树
            findUnique(nums,i+1,visited,track);
            //撤销选择
            track.removeLast();
            visited[i] = false;
        }
    }
```

## [38. 外观数列](https://leetcode-cn.com/problems/count-and-say/)

> #### [38. 外观数列](https://leetcode-cn.com/problems/count-and-say/)
>
> 难度简单519
>
> 给定一个正整数 *n*（1 ≤ *n* ≤ 30），输出外观数列的第 *n* 项。
>
> 注意：整数序列中的每一项将表示为一个字符串。
>
> 「外观数列」是一个整数序列，从数字 1 开始，序列中的每一项都是对前一项的描述。前五项如下：
>
> ```
> 1.     1
> 2.     11
> 3.     21
> 4.     1211
> 5.     111221
> ```

```java
	// 1  >  1
    // 2  > 11
    // 3  > 21
    // 4  > 1211
    public String countAndSay(int n) {
        if(n == 1){
            return "1";
        }
        StringBuffer res = new StringBuffer();
        String str = countAndSay(n-1);
        int length = str.length();
        int a = 0;
        for(int i=1;i<length+1;i++){
            // 当i == length 返回 添加上一个子部分的最后元素 
            if(i == length){
                return res.append(i-a).append(str.charAt(a)).toString();
            }else if(str.charAt(i) != str.charAt(a)){
                res.append(i - a).append(str.charAt(a));
                a = i;
            }
        }
        return res.toString();
    }
```

