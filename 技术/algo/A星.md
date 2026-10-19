

## [773. 滑动谜题](https://leetcode-cn.com/problems/sliding-puzzle/)

> 难度困难85
>
> 在一个 2 x 3 的板上（`board`）有 5 块砖瓦，用数字 `1~5` 来表示, 以及一块空缺用 `0` 来表示.
>
> 一次移动定义为选择 `0` 与一个相邻的数字（上下左右）进行交换.
>
> 最终当板 `board` 的结果是 `[[1,2,3],[4,5,0]]` 谜板被解开。
>
> 给出一个谜板的初始状态，返回最少可以通过多少次移动解开谜板，如果不能解开谜板，则返回 -1 。
>
> **示例：**
>
> ```
> 输入：board = [[1,2,3],[4,0,5]]
> 输出：1
> 解释：交换 0 和 5 ，1 步完成
> ```

```java
	// 0坐标对应向右交换和向下交换
    // 变化向量
    int[][] exchangeArray = new int[][]{
        {1, 3},
        {0, 2, 4},
        {1, 5},
        {0, 4},
        {1, 3, 5},
        {2, 4}
    };
    // 交换字符
    public String exchangeString(String string, int src, int dis) {
        char[] chars = string.toCharArray();
        char temp = chars[dis];
        chars[dis] = chars[src];
        chars[src] = temp;
        return new String(chars);
    }

    public int slidingPuzzle(int[][] board) {
        // 初始状态转字符串
        char[] chars = new char[6];
        int index = 0;
        for (int[] row:board) {
            for (int ch:row) {
                chars[index++] = (char)(ch+'0');
            }
        }
        String start = new String(chars);
        String target = "123450";//最终状态
        // BFS套路
        Queue<String> q = new ArrayDeque<>();
        Set<String> visited = new HashSet<>();
        q.offer(start);
        int step = 0;
        while (!q.isEmpty()) {
            int sz = q.size();
            for (int i = 0; i < sz; i++) {
                String cur = q.poll();
                // 解开谜板
                if (cur.equals(target)) {
                    return step;
                }
                int position = cur.indexOf('0');
                int[] exchanges = exchangeArray[position];
                for(int next:exchanges) {
                    String s = exchangeString(cur, position, next);
                    if (!visited.contains(s)) {
                        q.offer(s);
                        visited.add(s);
                    }
                }
            }
            step++;
        }
        return -1;
    }
```

