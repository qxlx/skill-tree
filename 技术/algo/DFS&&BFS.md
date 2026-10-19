# DFS&&BFS

## 102.二叉树的层序遍历

> #### [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)
>
> 难度中等460
>
> 给你一个二叉树，请你返回其按 **层序遍历** 得到的节点值。 （即逐层地，从左到右访问所有节点）。
>
>  
>
> **示例：**
> 二叉树：`[3,9,20,null,null,15,7]`,
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

### 1.DFS

```java
	List<List<Integer>> result = new ArrayList<>();
    //1.DFS 将每一次用k记录 添加到对应的层级
	// time :O(n) 
    public List<List<Integer>> levelOrder(TreeNode root) {
        if(root == null)    return result;
        recur(root,0);
        return result;
    }

    private void recur(TreeNode root,int k){
        if(root != null){
            if(result.size()<=k){
                result.add(new ArrayList<>());
            }
            result.get(k).add(root.val);
            recur(root.left,k+1);
            recur(root.right,k+1);
        }
    }	
```

### 2.BFS

```java
//2.BFS 层序遍历 将每层节点添加到queue中 
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if(root == null)    return result;
        Queue<TreeNode> queue = new LinkedList<>();

        //1.添加首节点
        queue.add(root);
        while(!queue.isEmpty()){
            int cnt = queue.size();
            List<Integer> list = new ArrayList<>();
            while(cnt-->0){
                TreeNode node = queue.poll();
                if(node == null){
                    continue;
                }
                list.add(node.val);
                queue.add(node.left);
                queue.add(node.right);
            }
            if(queue.size()!=0){
                result.add(list);
            }
        }
        return result;
    }
```

## [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/) ☆

> #### [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)
>
> 难度中等574
>
> 给你一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。
>
> 岛屿总是被水包围，并且每座岛屿只能由水平方向或竖直方向上相邻的陆地连接形成。
>
> 此外，你可以假设该网格的四条边均被水包围。
>
>  
>
> **示例 1:**
>
> ```
> 输入:
> 11110
> 11010
> 11000
> 00000
> 输出: 1
> ```
>
> **示例 2:**
>
> ```
> 输入:
> 11000
> 11000
> 00100
> 00011
> 输出: 3
> 解释: 每座岛屿只能由水平和/或竖直方向上相邻的陆地连接而成。
> ```

### dfs

```java
/*
     遍历岛屿 
     a.当遇到1 将岛屿变成2 依次将连着的岛屿 上下左右都改变、
     b.判断上下界限就可以了。
     time : O(M*N)  
     space O(1)
    */
    public int numIslands(char[][] grid) {
        int res = 0;
        for(int i=0;i<grid.length;i++){
            for(int j=0;j<grid[0].length;j++){
                if(grid[i][j] == '1'){//发生了多少次就有几个岛屿
                    res++;
                    dfs(grid,i,j);
                }
            }
        }
        return res;
    }

    private void dfs(char [][] grid,int i,int j){
        if(i < 0 || j < 0 || i >= grid.length || j >= grid[0].length || grid[i][j] != '1') return;
        grid[i][j] = '2';//防止再次递归的过程。感染一下。
        dfs(grid,i-1,j);
        dfs(grid,i+1,j);
        dfs(grid,i,j-1);
        dfs(grid,i,j+1);
    }
```

## [130. 被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)  ☆

> #### [130. 被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)
>
> 难度中等258
>
> 给定一个二维的矩阵，包含 `'X'` 和 `'O'`（**字母 O**）。
>
> 找到所有被 `'X'` 围绕的区域，并将这些区域里所有的 `'O'` 用 `'X'` 填充。
>
> **示例:**
>
> ```
> X X X X
> X O O X
> X X O X
> X O X X
> ```
>
> 运行你的函数后，矩阵变为：
>
> ```
> X X X X
> X X X X
> X X X X
> X O X X
> ```

```java
	//解题思路
    //dfs 1.通过从边界的 O 上下左右查找，如果可以找到说明 查找到的O可以连通到边界的O
    //所以 只需要 O 记录一下 修改成 # 标识一下。
    //如果最终通过边界的 O dfs完毕，还有剩余的O 说明没有找到。是被X包围的，就设置为X
    //时间复杂度 O(M*N) 空间复杂度O(1)

    public void solve(char[][] board) {
        if(board == null || board.length == 0){
            return;
        }

        int m = board.length;
        int n = board[0].length;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                boolean isEdge = i == 0 || j == 0 || i == m-1 || j == n-1;
                if(isEdge && board[i][j] == 'O'){
                    dfs(board,i,j);
                }
            }
        }

        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                //如果是 'O' 说明之前的dfs没有找到，是被'x'包围的
                if(board[i][j] == 'O'){
                    board[i][j] = 'X';
                }
                //之前dfs可以找到的'O' 
                if(board[i][j] == '#'){
                    board[i][j] = 'O';
                }
            }
        }
    }

    public void dfs(char [][] board,int i,int j){
        if(i < 0 || j < 0 || i >= board.length || j >= board[0].length || board[i][j] == 'X' || board[i][j] == '#'){
            return;
        }

        board[i][j] = '#';
        dfs(board,i-1,j);
        dfs(board,i+1,j);
        dfs(board,i,j-1);
        dfs(board,i,j+1);
    }
```

## [289. 生命游戏](https://leetcode-cn.com/problems/game-of-life/)

> #### [289. 生命游戏](https://leetcode-cn.com/problems/game-of-life/)
>
> 难度中等234
>
> 根据 [百度百科](https://baike.baidu.com/item/生命游戏/2926434?fr=aladdin) ，生命游戏，简称为生命，是英国数学家约翰·何顿·康威在 1970 年发明的细胞自动机。
>
> 给定一个包含 m × n 个格子的面板，每一个格子都可以看成是一个细胞。每个细胞都具有一个初始状态：1 即为活细胞（live），或 0 即为死细胞（dead）。每个细胞与其八个相邻位置（水平，垂直，对角线）的细胞都遵循以下四条生存定律：
>
> 1. 如果活细胞周围八个位置的活细胞数少于两个，则该位置活细胞死亡；
> 2. 如果活细胞周围八个位置有两个或三个活细胞，则该位置活细胞仍然存活；
> 3. 如果活细胞周围八个位置有超过三个活细胞，则该位置活细胞死亡；
> 4. 如果死细胞周围正好有三个活细胞，则该位置死细胞复活；
>
> 根据当前状态，写一个函数来计算面板上所有细胞的下一个（一次更新后的）状态。下一个状态是通过将上述规则同时应用于当前状态下的每个细胞所形成的，其中细胞的出生和死亡是同时发生的。
>
>  
>
> **示例：**
>
> ```
> 输入： 
> [
>   [0,1,0],
>   [0,0,1],
>   [1,1,1],
>   [0,0,0]
> ]
> 输出：
> [
>   [0,0,0],
>   [1,0,1],
>   [0,1,1],
>   [0,1,0]
> ]
> ```



```java
int [] dx = {-1,1,0,0,-1,-1,1,1};
    int [] dy = {0,0,-1,1,-1,1,-1,1};

    int [][] board;
    int m,n;
    // 1.规定    a.00为死亡到死亡 b.01为存活到死亡  c.10为死亡到存活 d.11 存活到存活
    // 为什么这样设定 因为不管之前什么状态 最终都是死亡状态的时候 通过位移1个位置就可以拿到最终的存活状态
    // 00>1 = 0  01>1 = 0  10>1 = 1  11>1 = 1
    // 是需要通过遍历查找每个位置的8个方位的存活状态 来决定当前是否是存活状态。
    // 时间复杂度O(N^2) 严格意义上来说，时间复杂度比O(N^2)多 因为每次都要查找当前位置的8个方位。
    public void gameOfLife(int[][] board) {
        if(board == null || board.length == 0 || board[0] == null || board[0].length == 0){
            return;
        }
        this.board = board;
        m = board.length;
        n = board[0].length;

        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                int cnt = countLive(i,j);
                // a.如果当前是board[i][j] 为存活状态，
                // 那么只有当cnt == 2 || cnt == 3的时候才可以存活 就设置为 3
                if((board[i][j] == 1) && (cnt == 2 || cnt == 3)){
                    board[i][j] = 3;
                }
                // b.如果当前是board[i][j] 为死亡状态
                // 那么当cnt == 3 就可以由死亡状态到存活状态
                if((board[i][j] == 0) &&(cnt == 3)){
                    board[i][j] = 2;
                }
            }
        }

        //将存活状态恢复
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                board[i][j] >>=1;
            }
        }
    }
    // 计算i,j 当前8个位置存活的个数。
    public int countLive(int i,int j){
        int sum = 0;
        for(int k=0;k<8;k++){
            int x = i+dx[k];
            int y = j+dy[k];
            //如果是越界 直接跳过
            if(x<0 || x >= m || y<0 || y >= n) continue;
            // 1.为什么只需要考虑&1就可以得出当前某一个方位的存活状态
            // 如果之前是死亡状态 0&1=0 如果之前是存活状态 1&1=1
            // 但是这块有一个问题，那就是我们在修改的时候，会出现问题的。比如你上一个方位是0
            sum += (board[x][y]&1);
        }
        return sum;
    }
```

