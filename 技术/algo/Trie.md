## [212. 单词搜索 II](https://leetcode-cn.com/problems/word-search-ii/)

> #### [212. 单词搜索 II](https://leetcode-cn.com/problems/word-search-ii/)
>
> 难度困难207
>
> 给定一个二维网格 **board** 和一个字典中的单词列表 **words**，找出所有同时在二维网格和字典中出现的单词。
>
> 单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母在一个单词中不允许被重复使用。
>
> **示例:**
>
> ```
> 输入: 
> words = ["oath","pea","eat","rain"] and board =
> [
>   ['o','a','a','n'],
>   ['e','t','a','e'],
>   ['i','h','k','r'],
>   ['i','f','l','v']
> ]
> 
> 输出: ["eat","oath"]
> ```



```java
	Set<String> res = new HashSet<String>();
	//a.先将words中单词添加到Trie中
	//b.遍历二维数组中的单词
    public List<String> findWords(char[][] board, String[] words) {
        Trie trie = new Trie();
        for(String word : words){
            trie.insert(word);
        }

        int m = board.length;
        int n = board[0].length;

        boolean [][]  visited = new boolean [m][n];
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                //board 二维数组
                //visited是否已经访问过
                //
                dfs(board,visited,"",i,j,trie);
            }
        }
        return new ArrayList<String>(res);
    }

    public void dfs(char [][] board,boolean [][] visited,String str,int x,int y,Trie trie){
        //边界处理
        if(x<0 || x>=board.length || y<0 || y>= board[0].length){
            return;
        }
        //访问过
        if(visited[x][y]){
            return;
        }
		
        str+=board[x][y];
        if(!trie.startsWith(str)) return;
        if(trie.search(str)){
            res.add(str);
        }

        visited[x][y] = true;
        dfs(board,visited,str,x-1,y,trie);
        dfs(board,visited,str,x+1,y,trie);
        dfs(board,visited,str,x,y-1,trie);
        dfs(board,visited,str,x,y+1,trie);
        visited[x][y] = false;
    }

}

class Trie {
    /**
     1.Trie树 是一个多路查找树，用于比较字符之间快速查找  是一种用空间换时间的思想。
     2，主要实现 内部有一个Trie数组 初始化大小为init_size 一个是否为字符串终止的标记
       a.添加 将字符串转换成字符数组，并将字符数组添加到对应的位置上。如果为null 直接new
       b.查找 和 前缀查找类似  前缀查找不需要判断是否为终止符  而查找需要在return 的时候 判断是否是终止符
    */
    private final int INIT_SIZE = 26;
    private Trie  [] ch = new Trie  [INIT_SIZE];
    private boolean isEndOfStr = false;
    /** Initialize your data structure here. */
    public Trie() {}
    public void insert(String word) {
        Trie tmp = this;
        for(char ch : word.toCharArray()){
            if(tmp.ch[ch-'a'] == null){
                tmp.ch[ch-'a'] = new Trie(); 
            }
            tmp = tmp.ch[ch-'a'];
        }
        tmp.isEndOfStr = true;
    }
    public boolean search(String word) {
        Trie tmp = this;
        for(char ch : word.toCharArray()){
            if(tmp.ch[ch-'a'] == null){
                return false;
            }
            tmp = tmp.ch[ch-'a'];
        }
        return tmp.isEndOfStr == true ? true : false;
    }
    public boolean startsWith(String prefix) {
        Trie tmp = this;
        for(char ch: prefix.toCharArray()){
            if(tmp.ch[ch-'a'] == null){
                return false;
            }
            tmp = tmp.ch[ch-'a'];
        }
        return true;
    }
}
```

## [208. 实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

> 难度中等373
>
> 实现一个 Trie (前缀树)，包含 `insert`, `search`, 和 `startsWith` 这三个操作。
>
> **示例:**
>
> ```
> Trie trie = new Trie();
> 
> trie.insert("apple");
> trie.search("apple");   // 返回 true
> trie.search("app");     // 返回 false
> trie.startsWith("app"); // 返回 true
> trie.insert("app");   
> trie.search("app");     // 返回 true
> ```

```java
/**
     1.Trie树 是一个多路查找树，用于比较字符之间快速查找  是一种用空间换时间的思想。
     2，主要实现 内部有一个Trie数组 初始化大小为init_size 一个是否为字符串终止的标记
       a.添加 将字符串转换成字符数组，并将字符数组添加到对应的位置上。如果为null 直接new
       b.查找 和 前缀查找类似  前缀查找不需要判断是否为终止符  而查找需要在return 的时候 判断是否是终止符
    */
    private final int INIT_SIZE = 26;
    private Trie  [] ch = new Trie  [INIT_SIZE];
    private boolean isEndOfStr = false;
    /** Initialize your data structure here. */
    public Trie() {}
    
    public void insert(String word) {
        Trie tmp = this;
        for(char ch : word.toCharArray()){
            //如果为空 直接创建一个新的Trie()
            if(tmp.ch[ch-'a'] == null){
                tmp.ch[ch-'a'] = new Trie(); 
            }
            tmp = tmp.ch[ch-'a'];
        }
        tmp.isEndOfStr = true;
    }

    public boolean search(String word) {
        Trie tmp = this;
        for(char ch : word.toCharArray()){
            if(tmp.ch[ch-'a'] == null){
                return false;
            }
            tmp = tmp.ch[ch-'a'];
        }
        //如果等于结尾表示结束
        return tmp.isEndOfStr == true ? true : false;
    }

	//查看是否具有某一个前缀树
    public boolean startsWith(String prefix) {
        Trie tmp = this;
        for(char ch: prefix.toCharArray()){
            if(tmp.ch[ch-'a'] == null){
                return false;
            }
            tmp = tmp.ch[ch-'a'];
        }
        return true;
    }
```



