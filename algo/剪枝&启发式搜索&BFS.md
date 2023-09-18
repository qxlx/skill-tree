## [36. 有效的数独](https://leetcode-cn.com/problems/valid-sudoku/)

> 难度中等391
>
> 判断一个 9x9 的数独是否有效。只需要**根据以下规则**，验证已经填入的数字是否有效即可。
>
> 1. 数字 `1-9` 在每一行只能出现一次。
> 2. 数字 `1-9` 在每一列只能出现一次。
> 3. 数字 `1-9` 在每一个以粗实线分隔的 `3x3` 宫内只能出现一次。
>
> ![img](e:\pic\250px-Sudoku-by-L2G-20050714.svg-1597373844634.png)
>
> 上图是一个部分填充的有效的数独。
>
> 数独部分空格内已填入了数字，空白格用 `'.'` 表示。

剪枝

```java
	public boolean isValidSudoku(char[][] board) {
        //行
        boolean [][] row = new boolean[9][10];
        //列
        boolean [][] clo = new boolean[9][10];
        //board的下标
        boolean [][] box = new boolean[9][10];

        for (int i = 0; i < 9 ;i++){
            for (int j = 0; j < 9 ;j++){
                if (board[i][j] == '.'){
                    continue;
                }
                //提取数字
                int num = board[i][j] - '0';
                //计算第几格
                int boardIndex = (i / 3) * 3 + j / 3;
                
                if (row[i][num] || clo[j][num] || box[boardIndex][num]) {
                    return false;
                }

                row[i][num] = true;
                clo[j][num] = true;
                box[boardIndex][num] = true;
            }
        }
        return true;
    }
```



## [37. 解数独](https://leetcode-cn.com/problems/sudoku-solver/)

> 难度困难505
>
> 编写一个程序，通过已填充的空格来解决数独问题。
>
> 一个数独的解法需**遵循如下规则**：
>
> 1. 数字 `1-9` 在每一行只能出现一次。
> 2. 数字 `1-9` 在每一列只能出现一次。
> 3. 数字 `1-9` 在每一个以粗实线分隔的 `3x3` 宫内只能出现一次。
>
> 空白格用 `'.'` 表示。
>
> ![img](e:\pic\250px-Sudoku-by-L2G-20050714.svg-1598168177615.png)



```java
	public void solveSudoku(char[][] board) {
        if (board == null || board[0].length == 0)  return;
        solve(board);
    }

    public boolean solve(char[][] board) {
        for (int i=0; i<board.length;i++) {
            for (int j=0;j<board[0].length;j++) {
                if (board[i][j] == '.') {
                    for (char c = '1' ; c<= '9' ; c++) {
                        //是否合法
                        if (isValid(board,i,j,c)) {
                            //合法就赋值
                            board[i][j] = c;
                            //递归解决下一个子问题
                            if (solve(board)) {
                                return true;
                            }else {
                                board[i][j] = '.';
                            }
                        }
                    }
                    return false;
                }
            }
        }
        return true;
    }

    public boolean isValid(char[][] board,int row,int col,char c) {
        for (int i=0;i<9;i++) {
            if (board[i][col] != '.' && board[i][col] == c) return false;
            if (board[row][i] != '.' && board[row][i] == c) return false;
            if (board[3 * (row / 3) + i/3][3 * (col/3) + i % 3] !='.'
                && board[3 * (row / 3)+ i/3][3 * (col/3) +i % 3] == c) 
                return false;
        }
        return true;
    }
```

