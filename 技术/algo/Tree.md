[TOC]



# 树

## 94.二叉树的中序遍历 ☆

> 给定一个二叉树，返回它的中序 遍历。
>
> 示例:
>
> 输入: [1,null,2,3]
>    1
>     \
>      2
>     /
>    3
>
> 输出: [1,3,2]

### 1.递归法

我们知道二叉树的遍历有前序遍历 中序遍历 后序遍历。

前序遍历 :根左右

中序遍历:左根右

后序遍历:左右根

时间复杂度:O(n)。递归函数 T(n) = 2*T(n/2)+1 

空间辅助度:最坏情况下需要空间O(n),平均情况为O(log N)

```java
 public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList();
        helper(root,result);
        return result;
    }

    public static void helper(TreeNode root,List<Integer> result){
        if(root!=null){
            //左
            if(root.left!=null){
                helper(root.left,result);
            }
            //中
            result.add(root.val);
            //右
            if(root.right!=null){
                helper(root.right,result);
            }
        }
    }
```

### 2.基于栈的遍历

用一个栈接收访问的路径 因为栈是先进后出 所以最后访问的元素就是最先遍历的元素。先查找左节点 左节点完毕后 输出，查找右节点。

时间复杂度:O(n)

空间复杂度:O(n)

```java
 public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList();
        Stack<TreeNode> stack = new Stack();
        //获取根节点 
        TreeNode curr = root;
        //如果当前节点不为null or 栈不为null 说明还有节点没有遍历完
        while(curr != null || !stack.empty()){
            //先将左节点push 进
            while(curr!=null){
                stack.push(curr);
                curr = curr.left;
            }
            //后push的 先输出
            curr = stack.pop();
            result.add(curr.val);
            //查看当前节点的右节点 
            curr = curr.right;
        }

        return result;
    }
```



## 144.二叉树的前序遍历

> 给定一个二叉树，返回它的 前序 遍历。
>
>  示例:
>
> 输入: [1,null,2,3]  
>    1
>     \
>      2
>     /
>    3 
>
> 输出: [1,2,3]
>

### 1.递归法

```java
	public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList();
        helper(root,result);
        return result;
    }

    public static void helper(TreeNode root,List<Integer> result){
        if(root!=null){
            //根
            result.add(root.val);
            //左
            if(root.left != null){
                helper(root.left,result);
            }
            //右
            if(root.right != null){
                helper(root.right,result);
            }
        }
    }
```

### 2.基于栈的遍历

其实我们使用递归去输出二叉树的遍历，计算机也是利用栈进行操作，我们可以模拟栈进行操作、

栈是先进后出的线性结构。因此 前序遍历是根左右  应该先push rootNode 输出。剩下就是左右节点的遍历，应该先push右节点，然后在push左节点，这样在pop的时候 顺序就是左右。

时间复杂度:O(n) 相当于树进行了一次loop操作

空间复杂度:O(n) 用栈做临时空间存储。

```java
public List<Integer> preorderTraversal(TreeNode root) {
        if(root == null){
            return new ArrayList();
        }

        List<Integer> result = new ArrayList();
        Stack<TreeNode> stack = new Stack();

        //先将头结点条件进去
        stack.push(root);

        while(!stack.isEmpty()){
            TreeNode node = stack.pop();
            result.add(node.val);
            //添加右节点 因为栈是先进后出 而前序遍历是根左右 所以右节点陷进去最后出来
            if(node.right != null){
                stack.push(node.right);
            }
            //添加左节点 
            if(node.left != null){
                stack.push(node.left);
            }

        }
        return result;
    }
```

## 590.N叉数的后序遍历

> #### [590. N叉树的后序遍历](https://leetcode-cn.com/problems/n-ary-tree-postorder-traversal/)
>
> 难度简单58
>
> 给定一个 N 叉树，返回其节点值的*后序遍历*。
>
> 例如，给定一个 `3叉树` :
>
> ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/narytreeexample.png)
>
>  
>
> 返回其后序遍历: `[5,6,3,2,4,1]`.
>
>  

### 1.递归

时间复杂度:O(n)

空间复杂度:O(n)

```java
LinkedList<Integer> linkedList = new LinkedList();
    //1.一个LikedList链表存储数值
    //2.递归调用
    // a.如果根节点为null 返回
    // b.依次遍历根节点的孩子节点 
    public List<Integer> postorder(Node root) {
        recur(root);
        return linkedList;
    }

    public void recur(Node root){
        if(root == null) return;
        for(Node node : root.children){
            recur(node);
        }
        linkedList.add(root.val);
    }
```

### 2.栈迭代

时间复杂度:O(n)

空间复杂度:O(n)

```java
 	// 1.N叉树的后序遍历 左右根 
    // 2.用一个链表存储数据 每次添加首节点 依次往后移动
    // 3.将root节点push到栈中 遍历左右子节点。如果不为null 继续
    public List<Integer> postorder(Node root) {
        if(root == null) return new ArrayList();
        LinkedList<Integer> linkedList = new LinkedList();
        Stack<Node> stack = new Stack();
        stack.push(root);
        while(!stack.isEmpty()){
            root = stack.pop();
            linkedList.offerFirst(root.val);//先进排在最后
            for(Node node : root.children){
                stack.push(node);
            }
        }
        return linkedList;
    }
```

## 589.N叉树的前序遍历

> #### [589. N叉树的前序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/)
>
> 难度简单71
>
> 给定一个 N 叉树，返回其节点值的*前序遍历*。
>
> 例如，给定一个 `3叉树` :
>
>  
>
> ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/narytreeexample.png)
>
>  
>
> 返回其前序遍历: `[1,3,5,6,2,4]`。

### 1.递归

```java
//前序遍历  根左右  递归调用即可。
    //先将节点的值存储到linkedList中

    LinkedList<Integer> linkedList = null;
    public List<Integer> preorder(Node root) {
        linkedList = new LinkedList();
        return root == null ? new ArrayList() : recur(root);
    }

    public LinkedList<Integer> recur(Node root){
        if(root == null)  return null;
        linkedList.add(root.val);
        for(Node node : root.children){
            recur(node);
        }
        return linkedList;
    }
```

### 2.栈迭代

```java
	//迭代
    //前序遍历 根-左-右
    //1.将根节点存储到stack中
    //2.逆序将子节点添加到栈中 比如 push的顺序为 2 3 4  则输出的顺序为 4 3 2 
    public List<Integer> preorder(Node root) {
        if(root == null) return new ArrayList();
        List<Integer> list = new ArrayList();
        Stack<Node> stack = new Stack();

        stack.push(root);

        while(!stack.isEmpty()){
            root = stack.pop();
            list.add(root.val);
            for(int i=root.children.size()-1;i>=0;i--){
                stack.push(root.children.get(i));
            }
        }
        return list;
    }
```

## 429.N叉数的层序遍历

> #### [429. N叉树的层序遍历](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/)
>
> 难度中等76
>
> 给定一个 N 叉树，返回其节点值的*层序遍历*。 (即从左到右，逐层遍历)。
>
> 例如，给定一个 `3叉树` :
>
> ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/narytreeexample.png)
>
>  
>
> 返回其层序遍历:
>
> ```
> [
>      [1],
>      [3,2,4],
>      [5,6]
> ]
> ```

### 1.队列实现广度优先搜索

time:O(n)

space:O(n)

```java
	//1.用队列实现广度优先搜索
    //2.一个list 一个queue  list存储节点的值  queue存储每一层遍历的节点。
    //3.当queue != null 的时候，遍历当前层。
    public List<List<Integer>> levelOrder(Node root) {
        if(root == null) return new ArrayList();
        List<List<Integer>> list = new ArrayList<>();
        Queue<Node> queue = new LinkedList();//存储每一层的结点值
        queue.add(root);
        while(!queue.isEmpty()){
            List<Integer> level = new ArrayList<>();
            int size = queue.size();
            for(int i=0;i<size;i++){
                Node node = queue.poll();
                level.add(node.val);
                queue.addAll(node.children);
            }
            list.add(level);
        }
        return list;
    }
```



## [222. 完全二叉树的节点个数](https://leetcode-cn.com/problems/count-complete-tree-nodes/)

> #### [222. 完全二叉树的节点个数](https://leetcode-cn.com/problems/count-complete-tree-nodes/)
>
> 难度中等156
>
> 给出一个**完全二叉树**，求出该树的节点个数。
>
> **说明：**
>
> [完全二叉树](https://baike.baidu.com/item/%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91/7773232?fr=aladdin)的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层，则该层包含 1~ 2h 个节点。
>
> **示例:**
>
> ```
> 输入: 
>     1
>    / \
>   2   3
>  / \  /
> 4  5 6
>
> 输出: 6
> ```

```java
public int countNodes(TreeNode root) {
        if(root == null){
            return 0;
        }

        int left = countHigh(root.left);
        int right = countHigh(root.right);
        if(left == right){
            return countNodes(root.right) + (1<<left);
        }else{
            return countNodes(root.left) +(1<<right);
        }
    }

    //向左查找树的深度
    public int countHigh(TreeNode root){
        int count = 0;
        while(root!=null){
            count++;
            root = root.left;
        }
        return count;
    }

```



## 101.对称二叉树

> #### [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)
>
> 难度简单882
>
> 给定一个二叉树，检查它是否是镜像对称的。
>
>  
>
> 例如，二叉树 `[1,2,2,3,4,4,3]` 是对称的。
>
> ```
>     1
>    / \
>   2   2
>  / \ / \
> 3  4 4  3
> ```
>
>  
>
> 但是下面这个 `[1,2,2,null,3,null,3]` 则不是镜像对称的:
>
> ```
>     1
>    / \
>   2   2
>    \   \
>    3    3
> ```

递归终止条件 当都是空指针 说明两者相等。 如果一个为null 一个不为null 说明两者不等返回false

递归去判断A的左子树和B的右子树  以及 A的右子树和B的左子树。其中短路与的作用决定如果一个子树不对称，那么整颗树是不对称的。

```java
	public boolean isSymmetric(TreeNode root) {
        if (root == null) return true;
        return isSymmetric(root.left,root.right);
    }

    private boolean isSymmetric(TreeNode r,TreeNode l) {
        if (r == null && l == null) return true;
        if (r == null || l == null) return false;
        if (r.val != l.val) return false;
        return isSymmetric(r.left,l.right) && isSymmetric(r.right,l.left);
    }
```



## 437.路径总和

> #### [437. 路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/)
>
> 难度简单472
>
> 给定一个二叉树，它的每个结点都存放着一个整数值。
>
> 找出路径和等于给定数值的路径总数。
>
> 路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。
>
> 二叉树不超过1000个节点，且节点数值范围是 [-1000000,1000000] 的整数。
>
> **示例：**
>
> ```
> root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8
> 
>       10
>      /  \
>     5   -3
>    / \    \
>   3   2   11
>  / \   \
> 3  -2   1
> 
> 返回 3。和等于 8 的路径有:
> 
> 1.  5 -> 3
> 2.  5 -> 2 -> 1
> 3.  -3 -> 11
> ```

题中给出的条件不从root开始 叶子节点结尾。那么需要分成3部分去查找。

1.从头结点查找  2.头结点的左子节点查找 3.头结点的右子节点查找

```java
	public int pathSum(TreeNode root, int sum) {
        if(root == null ){
            return 0;
        }
        //notes
        return pathCount(root,sum)+pathSum(root.left,sum)+pathSum(root.right,sum);
    }

    private int pathCount(TreeNode root,int sum){
        if(root == null){
            return 0;
        }
        sum -= root.val;
        int result = sum == 0 ? 1 : 0;
        return result+pathCount(root.left,sum)+pathCount(root.right,sum);
    }	
```

## 538.把二叉搜索树转换为累加树 ☆

> #### [538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)
>
> 难度简单275
>
> 给定一个二叉搜索树（Binary Search Tree），把它转换成为累加树（Greater Tree)，使得每个节点的值是原来的节点值加上所有大于它的节点值之和。
>
>  
>
> **例如：**
>
> ```
> 输入: 原始二叉搜索树:
>               5
>             /   \
>            2     13
> 
> 输出: 转换为累加树:
>              18
>             /   \
>           20     13
> ```

我们分析一下 原来BST的结果为 [2,5,13] 转换后为[20,18,13] 而[13,18,20]只是他的逆序。  也就是中序遍历的逆序、累加上sum，因此直接用中序遍历的逆序即可。先遍历右子节点 保存当前值，在遍历左子节点。

```java
	int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        if(root!=null){
            convertBST(root.right);
            sum += root.val;
            root.val = sum;
            convertBST(root.left);
        }
        return root;
    }
```



## 543.二叉树的直径

> #### [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)
>
> 难度简单397
>
> 给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。
>
>  
>
> **示例 :**
> 给定二叉树
>
> ```
>           1
>          / \
>         2   3
>        / \     
>       4   5    
> ```
>
> 返回 **3**, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。

### 1.dfs

最长直径。可能有三种情况、1.要么在左子树中 2.要么在右子树中 3.要么经过根节点、

前两者直接递归 求出最大值即可。但是第三种情况，我们需要存储一个变量。也就是当前递归的最大值。

至于最终返回的是max 而不是Math.max(leftDepth,rightDepth)+1;  是因为max存储的值最长直径，而Math.max(leftDepth,rightDepth)+1;存储的只是当前节点的左右子树的最大值+1。

时间复杂度为O(n)

空间复杂度为O(1)

```java
	private int max = 0;//存储最长直径

    public int diameterOfBinaryTree(TreeNode root) {
        dfs(root);
        return max;
    }

    public int dfs(TreeNode root){
        if(root == null){
            return 0;
        }
        int leftDepth = dfs(root.left);//递归左子树长度
        int rightDepth = dfs(root.right);//递归右子树长度
        max = Math.max(leftDepth+rightDepth,max);//二者最大值
        
        return Math.max(leftDepth,rightDepth)+1;
    }
```



## 617.合并二叉树

> #### [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)
>
> 难度简单412
>
> 给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。
>
> 你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则**不为** NULL 的节点将直接作为新二叉树的节点。
>
> **示例 1:**
>
> ```
> 输入: 
> 	Tree 1                     Tree 2                  
>           1                         2                             
>          / \                       / \                            
>         3   2                     1   3                        
>        /                           \   \                      
>       5                             4   7                  
> 输出: 
> 合并后的树:
> 	     3
> 	    / \
> 	   4   5
> 	  / \   \ 
> 	 5   4   7
> ```

### 1.dfs

时间复杂度为O(n)

空间复杂度为O(1)

```java
	public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
	//如果某一个树的根节点为Null 返回另外一个，否则的话 返回Null
        if(t1 == null || t2 == null){
            return t1 == null ? t2 : t1;
        }
        return dfs(t1,t2);
    }

    public TreeNode dfs(TreeNode r1,TreeNode r2){
        //终止条件 
        if(r1 == null ||r2 == null){
            return r1 == null ? r2 : r1;
        }
        //r1.val 加上 r2.val
        r1.val+=r2.val;
        r1.left = dfs(r1.left,r2.left);
        r1.right = dfs(r1.right,r2.right);
        return r1;
    }
```

### 2.Queue

左右子节点都不为Null的时候，直接添加到队列中，如果r1的左节点为Null  r2的左节点不为Null  直接赋值

但是当r1的左节点不为Null  r2的右节点不为Null 就不需要修改值。

时间复杂度为O(n)

空间复杂度为O(1) 对于满二叉树来说，要保存所有的叶子节点。N/2

```java
	public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if(t1 == null || t2 == null){
            return t1 == null ? t2 : t1;
        }
        LinkedList<TreeNode> queue = new LinkedList<>();
        queue.add(t1);
        queue.add(t2);
        while(queue.size()>0){
            TreeNode r1 = queue.remove();
            TreeNode r2 = queue.remove();
            r1.val +=r2.val;
            //如果左右子节点不为null
            if(r1.left!=null&&r2.left!=null){
                queue.add(r1.left);
                queue.add(r2.left);
            }else if(r1.left == null){
                //如果左子节点null 右子节点不为null
                r1.left =  r2.left;
            }

            //右子树
            if(r1.right!=null&&r2.right!=null){
                queue.add(r1.right);
                queue.add(r2.right);
            }else if(r1.right == null){
                r1.right = r2.right;
            }
        }
        return t1;
    }
```

## [108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

> #### [108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)
>
> 难度简单510
>
> 将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。
>
> 本题中，一个高度平衡二叉树是指一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过 1。
>
> **示例:**
>
> ```
> 给定有序数组: [-10,-3,0,5,9],
> 
> 一个可能的答案是：[0,-3,9,-10,null,5]，它可以表示下面这个高度平衡二叉搜索树：
> 
>       0
>      / \
>    -3   9
>    /   /
>  -10  5
> ```

### 分治

```java
 /*
    ** 思路 有序数组转换成二叉搜索树，我们只需要构建某一个节点，递归构建左右子节点就可以。
    但是题目要求的是必须要求高度差不能超过1,所以根节点值的选取就要折中。
    BST中序遍历是一个有序数组，可以使用mid为root节点。然后分治的思想 递归构建左右子节点。
    时间复杂度为O(n) 空间复杂度为O(logN) 递归栈调用的深度
    */
    public TreeNode sortedArrayToBST(int[] nums) {
        if(nums == null || nums.length == 0){
            return null;
        }
        return dfs(nums,0,nums.length-1);
    }

    public TreeNode dfs(int [] nums,int left,int right){
        if(left>right){
            return null;
        }
        int mid =left+(right-left) / 2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left =  dfs(nums,left,mid-1);
        root.right = dfs(nums,mid+1,right);
        return root;
    }
```

## [124. 二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

> #### [124. 二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)
>
> 难度困难595
>
> 给定一个**非空**二叉树，返回其最大路径和。
>
> 本题中，路径被定义为一条从树中任意节点出发，达到任意节点的序列。该路径**至少包含一个**节点，且不一定经过根节点。
>
> **示例 1:**
>
> ```
> 输入: [1,2,3]
> 
>        1
>       / \
>      2   3
> 
> 输出: 6
> ```



```java
	private int res = Integer.MIN_VALUE;

    public int maxPathSum(TreeNode root) {
        if(root == null){
            return res;
        }
        dfs(root);
        return res;
    }

    public int dfs(TreeNode root){
        if(root == null){
            return 0;
        }
        //选取左右子树的最大值
        int left = Math.max(0,dfs(root.left));
        int right = Math.max(0,dfs(root.right));
        //将最大值存储到res中
        res = Math.max(res,root.val+left+right);
        //返回当前最大的子树和+root.val
        return root.val+Math.max(left,right);
    }
```

## [114. 二叉树展开为链表](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)

> #### [114. 二叉树展开为链表](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)
>
> 难度中等420
>
> 给定一个二叉树，[原地](https://baike.baidu.com/item/原地算法/8010757)将它展开为一个单链表。
>
>  
>
> 例如，给定二叉树
>
> ```
>     1
>    / \
>   2   5
>  / \   \
> 3   4   6
> ```
>
> 将其展开为：
>
> ```
> 1
>  \
>   2
>    \
>     3
>      \
>       4
>        \
>         5
>          \
>           6
> ```

// 树的解决方式dfs(先序遍历，中序遍历，后序遍历) 
// bds(层序遍历) 
// 而本题只能用dfs 通过交换左右子树的位置
// 时间复杂度为O(n)  空间复杂度O(1)

```java
	// 树的解决方式dfs(先序遍历，中序遍历，后序遍历) 
    // bds(层序遍历) 
    // 而本题只能用dfs 通过交换左右子树的位置
    // 时间复杂度为O(n)  空间复杂度O(1)
    public void flatten(TreeNode root) {
        if(root == null){
            return;
        }
        //递归左子树
        flatten(root.left);
        //递归右子树
        flatten(root.right);
        //交换节点
        TreeNode tmp = root.right;
        root.right = root.left;
        //需要设置为空的原因，如果不设置为Null。那么当递归函数返回的时候，就会造成重复节点的交换。
        root.left = null;

        while(root.right!=null){
            root = root.right;
        }
        root.right = tmp;
    }
```

## [100. 相同的树](https://leetcode-cn.com/problems/same-tree/)

> #### [100. 相同的树](https://leetcode-cn.com/problems/same-tree/)
>
> 难度简单424
>
> 给定两个二叉树，编写一个函数来检验它们是否相同。
>
> 如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。
>
> **示例 1:**
>
> ```
> 输入:       1         1
>           / \       / \
>          2   3     2   3
> 
>         [1,2,3],   [1,2,3]
> 
> 输出: true
> ```
>
> **示例 2:**
>
> ```
> 输入:      1          1
>           /           \
>          2             2
> 
>         [1,2],     [1,null,2]
> 
> 输出: false
> ```
>
> **示例 3:**
>
> ```
> 输入:       1         1
>           / \       / \
>          2   1     1   2
> 
>         [1,2,1],   [1,1,2]
> 
> 输出: false
> ```

```java
	public boolean isSameTree(TreeNode p, TreeNode q) {
        if(p == null && q == null){
            return true;
        }
        if(p == null || q == null){
            return false;
        }
        if(p.val != q.val){
            return false; 
        }
        return isSameTree(p.left,q.left) && isSameTree(p.right,q.right);
    }
```

## [99\. 恢复二叉搜索树](https://leetcode-cn.com/problems/recover-binary-search-tree/)

> ### [99\. 恢复二叉搜索树](https://leetcode-cn.com/problems/recover-binary-search-tree/)
>
> Difficulty: **困难**
>
>
> 二叉搜索树中的两个节点被错误地交换。
>
> 请在不改变其结构的情况下，恢复这棵树。
>
> **示例 1:**
>
> ```
> 输入: [1,3,null,null,2]
> 
>    1
>   /
>  3
>   \
>    2
> 
> 输出: [3,1,null,null,2]
> 
>    3
>   /
>  1
>   \
>    2
> ```
>
> **示例 2:**
>
> ```
> 输入: [3,1,4,null,null,2]
> 
>   3
>  / \
> 1   4
>    /
>   2
> 
> 输出: [2,1,4,null,null,3]
> 
>   2
>  / \
> 1   4
>    /
>   3
> ```
>
> **进阶:**
>
> *   使用 O(_n_) 空间复杂度的解法很容易实现。
> *   你能想出一个只使用常数空间的解决方案吗？
>
>
> #### Solution
>
> Language: **全部题目**
>
> ```全部题目
> 
> ```

### 1.迭代

```java
	// 对于树的解决方案，基本是都是在做遍历的时候 进行数据操作。因此，本题采用中序遍历。
    // 中序遍历是一个有序的 左中右。 那么对于一个无序的 需要我们修改的中序结果为 3,1,null,null,2
    // 可以分析到 3 1 是我们交换的位置，
    // 1.找到第一个 既 第一个大于后一个的值即为 firstNode 
    // 2.第二个 在找到第一个的前提下，前一个大于后一个。即为secondNode
    // 好了，我们来分析一下时间复杂度 即为O(n) 只需要遍历一遍树即可。
    public void recoverTree(TreeNode root) {
        if(root == null) return;
        TreeNode p1 = null;
        TreeNode p2 = null;
        TreeNode p = new TreeNode(Integer.MIN_VALUE);

        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode cur = root;
        
        while(cur != null || !stack.isEmpty()){
            while(cur != null){
                stack.push(cur);
                cur = cur.left;
            }
            cur = stack.pop();
            //找到第一个节点
            if(p1 == null && p.val > cur.val) p1 = p;
            //找到第二个节点
            if(p1 != null && p.val > cur.val) p2 = cur;
            p = cur;
            cur = cur.right;
        }
        int tmp = p1.val;
        p1.val = p2.val;
        p2.val = tmp;
    }
```

### 2.递归

```java
	TreeNode p1 = null;
    TreeNode p2 = null;
    TreeNode p = new TreeNode(Integer.MIN_VALUE);

    public void recoverTree(TreeNode root) {
        in_order(root);    
        int t = p1.val;
        p1.val = p2.val;
        p2.val = t;
    }

    public void in_order(TreeNode r){
        if(r == null) return;
        in_order(r.left);
        if(p1 == null && p.val > r.val) p1 = p;
        if(p1 != null && p.val > r.val) p2 = r;
        p = r;
        in_order(r.right);
    }
```





## [530. 二叉搜索树的最小绝对差](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/) ☆

> 难度简单192
>
> 给你一棵所有节点为非负值的二叉搜索树，请你计算树中任意两节点的差的绝对值的最小值。
>
>  
>
> **示例：**
>
> ```
> 输入：
> 
>    1
>     \
>      3
>     /
>    2
> 
> 输出：
> 1
> ```



```java
	//因为中序遍历是有序的，我们只需要在中序遍历的过程中 记录上一个节点
    //然后当前root节点进行比较 求出最小值就可以
    //时间复杂度O(N)
    TreeNode tmp = null;
    private int min = Integer.MAX_VALUE;

    public int getMinimumDifference(TreeNode root) {
        recur(root);
        return min;
    }

    private void recur (TreeNode root) {
        if ( root == null )   return;
        recur(root.left);
        if (tmp != null) 
            min = Math.min(min,root.val - tmp.val);
        tmp = root;
        recur(root.right);
    }
```



## [110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)  ☆

> 难度简单506
>
> 给定一个二叉树，判断它是否是高度平衡的二叉树。
>
> 本题中，一棵高度平衡二叉树定义为：
>
> > 一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过1。
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

```java
	//主要思路 在遍历的时候，通过判断左右子树的高度 来判断这个二叉树是否是平衡的
    //因为必须要进行遍历一遍才可以知道是否平衡 所以不算递归调用的时间
    //时间复杂度为O(n)
    private boolean isBalance = true;

    public boolean isBalanced(TreeNode root) {
        isBalan (root);
        return isBalance;
    }

    private int isBalan(TreeNode r) {
        if (r == null) return 0;
        //2  3
        int left = isBalan(r.left);
        int right = isBalan(r.right);
        if (Math.abs(left-right) > 1) {
            isBalance = false;
        }
        
        return Math.max(left,right)+1;
    }
```



## [404. 左叶子之和](https://leetcode-cn.com/problems/sum-of-left-leaves/)

> 难度简单247
>
> 计算给定二叉树的所有左叶子之和。
>
> **示例：**
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
> 
> 在这个二叉树中，有两个左叶子，分别是 9 和 15，所以返回 24
> ```

```java
	//a.要么当前节点没有子节点 可以说明当前节点为左节点，直接root.left.val + 右子树
    //b.否则做左子树和右子树都计算一下
    public int sumOfLeftLeaves(TreeNode root) {
       if (root == null) return 0;
       if (isLeft (root.left)) return root.left.val + sumOfLeftLeaves(root.right);
       return sumOfLeftLeaves(root.left) + sumOfLeftLeaves(root.right);
    }

    private boolean isLeft (TreeNode r) {
        if (r == null) return false;
        return r.left == null && r.right == null;
    }
```

## [687. 最长同值路径](https://leetcode-cn.com/problems/longest-univalue-path/)

> 难度中等378
>
> 给定一个二叉树，找到最长的路径，这个路径中的每个节点具有相同值。 这条路径可以经过也可以不经过根节点。
>
> **注意**：两个节点之间的路径长度由它们之间的边数表示。
>
> **示例 1:**
>
> 输入:
>
> ```
>               5
>              / \
>             4   5
>            / \   \
>           1   1   5
> ```

```java
	//将问题抽象化，可以很容易的分析出，一条最长路径的情况
    //a.在左子树中 b.在右子树中 c.在整个树中
    //只需要计算出左子树和右子树 然后在和根节点进行比较 
    private int path = 0;

    public int longestUnivaluePath(TreeNode root) {
        dfs(root);
        return path;
    }

    private int dfs (TreeNode root) {
        if (root == null) return 0;
        int left = dfs (root.left);
        int right = dfs (root.right);
        int l = ( root.left != null) && ( root.left.val == root.val ) ? left + 1 : 0;
        int r = ( root.right != null)&& ( root.right.val == root.val )? right + 1 : 0;
        path = Math.max(path,l + r);
        return Math.max (l,r);
    }
```



## [637. 二叉树的层平均值](https://leetcode-cn.com/problems/average-of-levels-in-binary-tree/)

> 难度简单204
>
> 给定一个非空二叉树, 返回一个由每层节点平均值组成的数组。
>
>  
>
> **示例 1：**
>
> ```
> 输入：
>     3
>    / \
>   9  20
>     /  \
>    15   7
> 输出：[3, 14.5, 11]
> 解释：
> 第 0 层的平均值是 3 ,  第1层是 14.5 , 第2层是 11 。因此返回 [3, 14.5, 11] 。
> ```

```java
	public List<Double> averageOfLevels(TreeNode root) {
        List<Double> ret = new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);

        while (!queue.isEmpty()) {
            int size = queue.size();
            double sum = 0;
            int length = size;
            while (--size >= 0) {
                TreeNode r = queue.poll();
                sum += r.val;
                if (r.left != null) queue.add(r.left);
                if (r.right != null) queue.add(r.right);
            }
            ret.add(sum/length);
        }
        return ret;
    }
```

## [513. 找树左下角的值](https://leetcode-cn.com/problems/find-bottom-left-tree-value/)

> 难度中等124
>
> 给定一个二叉树，在树的最后一行找到最左边的值。
>
> **示例 1:**
>
> ```
> 输入:
> 
>     2
>    / \
>   1   3
> 
> 输出:
> 1
> ```

```java
 	//普通的遍历 借助于栈的 我们只需要将添加顺序改成从右往左。
    //我们来分析一下，如果一个正常的二叉树，那么队列中剩余的元素，一定是就是
    //最左元素，那么如果只有左子树，右子树不会被添加到，
    //分析一下时间复杂度为O(N) 只需要遍历一遍二叉树就可以找到
    public int findBottomLeftValue(TreeNode root) {
        if (root == null) return -1;
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.add(root);
        int ret = -1;
        while (!queue.isEmpty()) {
            int size = queue.size();
            while (--size >= 0) {
                TreeNode r = queue.poll();
                ret = r.val;
                if (r.right != null) queue.add(r.right);
                if (r.left != null) queue.add(r.left);
            }
        }
        return ret;
    }
```



## [671. 二叉树中第二小的节点](https://leetcode-cn.com/problems/second-minimum-node-in-a-binary-tree/)

> 难度简单118
>
> 给定一个非空特殊的二叉树，每个节点都是正数，并且每个节点的子节点数量只能为 `2` 或 `0`。如果一个节点有两个子节点的话，那么该节点的值等于两个子节点中较小的一个。
>
> 更正式地说，`root.val = min(root.left.val, root.right.val)` 总成立。
>
> 给出这样的一个二叉树，你需要输出所有节点中的**第二小的值。**如果第二小的值不存在的话，输出 -1 **。**
>
>  

```java
	public int findSecondMinimumValue(TreeNode root) {
        if (root == null) return -1;
        return recur (root,root.val);
    }
    //因为val传递的root.val 当出现第一个不等于val的值 一定就是次小者的值
    private int recur (TreeNode r,int val) {
        if (r.val != val) return r.val;
        //因为子节点只会存在0或2个，如果左子节点为null
        //说明是不存在叶子节点的
        if (r.left == null) return -1;
        int left = -1, right = -1;
        //递归求解
        if (r.left != null) left = recur(r.left,val);
        if (r.right != null) right = recur(r.right,val);
        if (left == -1) return right;
        if (right == -1) return left;
        //求二者差值
        return Math.min(left,right);
    }
```



## [230. 二叉搜索树中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/)  ☆

> 难度中等302
>
> 给定一个二叉搜索树，编写一个函数 `kthSmallest` 来查找其中第 **k** 个最小的元素。
>
> **说明：**
> 你可以假设 k 总是有效的，1 ≤ k ≤ 二叉搜索树元素个数。
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
> 输出: 1
> ```

```java
	private int val;
    private int count  = 1;

    //直接使用中序遍历 记录当前的状态值 时间复杂度O(n)
    public int kthSmallest(TreeNode root, int k) {
        recur(root,k);
        return val;
    }

    private void recur(TreeNode r,int k) {
        if (r == null) return;
        recur(r.left,k);
        if (count++ == k) {
            val = r.val;
            return;
        }
        recur(r.right,k);
    }
```



## [669. 修剪二叉搜索树](https://leetcode-cn.com/problems/trim-a-binary-search-tree/)

> 难度简单295
>
> 给你二叉搜索树的根节点 `root` ，同时给定最小边界`low` 和最大边界 `high`。通过修剪二叉搜索树，使得所有节点的值在`[low, high]`中。修剪树不应该改变保留在树中的元素的相对结构（即，如果没有被移除，原有的父代子代关系都应当保留）。 可以证明，存在唯一的答案。
>
> 所以结果应当返回修剪好的二叉搜索树的新的根节点。注意，根节点可能会根据给定的边界发生改变。
>
>  
>
> **示例 1：**
>
> ![img](e:\pic\trim1.jpg)
>
> ```
> 输入：root = [1,0,2], low = 1, high = 2
> 输出：[1,null,2]
> ```

```java
	//考虑递归问题，一般我们抽象化，从顶层考虑问题
    //利用根节点来判断，左右子节点是否需。
    //可能出现三种情况
    //a.当前节点大于最高值 直接选举左子树
    //b.当前节点小于最小值 直接选举右子树
    //c.求出左右子节点
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if (root == null) return null;
        if (root.val > high) return trimBST(root.left,low,high);
        if (root.val < low) return trimBST(root.right,low,high);
        root.left = trimBST(root.left,low,high);
        root.right = trimBST(root.right,low,high);
        return root;
    }
```



## [653. 两数之和 IV - 输入 BST](https://leetcode-cn.com/problems/two-sum-iv-input-is-a-bst/)

> 难度简单190
>
> 给定一个二叉搜索树和一个目标结果，如果 BST 中存在两个元素且它们的和等于给定的目标结果，则返回 true。
>
> **案例 1:**
>
> ```
> 输入: 
>     5
>    / \
>   3   6
>  / \   \
> 2   4   7
> 
> Target = 9
> 
> 输出: True
> ```

```java
	public boolean findTarget(TreeNode root, int k) {
        List<Integer> list = new ArrayList<>();
        if (root == null) return false;
        recur(root,list);
        int l = 0, r = list.size()-1;
        while (l < r) {
            int sum = list.get(l) + list.get(r);
            if (sum == k) {
                return true;
            } else if (sum > k) {
                r--;
            } else {
                l++;
            }
        }
        return false;
    }

    private void recur(TreeNode r,List<Integer> list) {
        if (r == null) return;
        recur(r.left,list);
        list.add(r.val);
        recur(r.right,list);
    }
```



## [109. 有序链表转换二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)

> 难度中等403
>
> 给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。
>
> 本题中，一个高度平衡二叉树是指一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过 1。
>
> **示例:**
>
> ```
> 给定的有序链表： [-10, -3, 0, 5, 9],
> 
> 一个可能的答案是：[0, -3, 9, -10, null, 5], 它可以表示下面这个高度平衡二叉搜索树：
> 
>       0
>      / \
>    -3   9
>    /   /
>  -10  5
> ```

```java
public TreeNode sortedListToBST(ListNode head) {
        //如果为null 直接返回null
        if (head == null) return null;
        //如果只有一个节点 直接返回一个节点
        if (head.next == null) return new TreeNode(head.val);
        //中间节点的前一个
        ListNode pre = preNode(head);
        //中间节点
        ListNode mid = pre.next;
        //链表断开 
        pre.next = null;
        //中间节点分开
        TreeNode t = new TreeNode(mid.val);
        
        t.left = sortedListToBST(head);
        t.right = sortedListToBST(mid.next);
        return t;
    }

    private ListNode preNode (ListNode head) {
        if (head == null) return null;
        ListNode pre = head;
        ListNode slow = head;
        ListNode fast = head.next;

        while (fast != null && fast.next != null) {
            pre = slow;
            slow = slow.next;
            fast = fast.next.next;
        }
        return pre;
    }

```



## [501. 二叉搜索树中的众数](https://leetcode-cn.com/problems/find-mode-in-binary-search-tree/)

> 难度简单235
>
> 给定一个有相同值的二叉搜索树（BST），找出 BST 中的所有众数（出现频率最高的元素）。
>
> 假定 BST 有如下定义：
>
> - 结点左子树中所含结点的值小于等于当前结点的值
> - 结点右子树中所含结点的值大于等于当前结点的值
> - 左子树和右子树都是二叉搜索树
>
> 例如：
> 给定 BST `[1,null,2,2]`,
>
> ```
>    1
>     \
>      2
>     /
>    2
> ```

```java
//必须进行初始化，当前值节点为null，会被之后节点删除
    private TreeNode preNode = new TreeNode(-1);
    private int max = 0;
    private Integer cur = 0;

    //中序遍历+剪枝
    public int[] findMode(TreeNode root) {
        if (root == null) return new int [0];
        List<Integer> list = new ArrayList<>();
        inOrder(root,list);
        int [] arr = new int [list.size()];
        int i = 0;
        for (int num : list) {
            arr[i++] = num;
        }
        return arr;
    }

    private void inOrder (TreeNode r,List<Integer> list) {
        if (r == null) return;
        inOrder(r.left,list);

        if (preNode != null) {
            //如果相等 之前的基础上++
            if (preNode.val == r.val) {
                cur++;
            } else {
                cur = 1;
            }
        }

        //一旦出现大于之前的 则删除集合中的元素 并修改max最大值
        if (cur > max) {
            max = cur;
            list.clear();
            list.add(r.val);
            //出现相等的直接添加到集合中
        } else if (cur == max) {
            list.add(r.val);
        }
        preNode = r;
        inOrder(r.right,list);
    }
```

