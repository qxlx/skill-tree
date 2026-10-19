

## 239.滑动窗口最大值

> #### [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)
>
> 难度困难290
>
> 给定一个数组 *nums*，有一个大小为 *k *的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 *k* 个数字。滑动窗口每次只向右移动一位。
>
> 返回滑动窗口中的最大值。
>
>  
>
> **进阶：**
>
> 你能在线性时间复杂度内解决此题吗？
>
>  
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

### 1.暴力法

思路：我们可以将滑动窗口问题抽象化，一直移动那么最后的数据格式就是一个二维数组 for for 遍历找出每次滑动窗口的最大值存储起来，就可以了。首先是确定滑动窗口的次数。k是窗口的大小，n是长度   k = 3   n = 8   移动了6    *size = n - k +1*   外层循环是移动次数 内层循环开始条件是 i  随着移动次数增加， k = i   终止条件为 最终移动的次数 i + k  就到数组的最后了。剩下依次判断就可以了。

时间复杂度：O(N*K)  外层k次 内层N次

空间复杂度：O(n-k+1) 

```java
public int[] maxSlidingWindow(int[] nums, int k) {
        //参数判断
        int length = nums.length;
        if(length * k == 0){
            return new int [0];
        }
        //窗口移动的次数为length-k+1
        int [] array = new int [length-k+1];  
        for(int i=0;i<length-k+1;i++){
            int max = Integer.MIN_VALUE;
          //因为窗口每次都要移动 在i的基础上 增加 终止条件是i+k 说明到底
          //每一次loop 找出最大值
            for(int j=i;j<i+k;j++){
                max = Math.max(max,nums[j]);
            }
           //该层的最大值
            array[i] = max;
        }
        return array;
    }
```

### 2.双端队列

思路:使用队列进行存储每次窗口的最大值的下标，如果当前值最大 那么之前的所有值就可以清空了

ps : 1 3 4    4最大 所以在这个窗口期中4一直最大 1 3 就可以出队列。 

time:O(N) 队列的出队和入队是O(1)  只存在遍历n次数组中的数据

space:O(n)  

```java
	public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums.length == 0 || k == 0) 
            return new int[0];
        Deque<Integer> deque = new LinkedList<>();
        int[] res = new int[nums.length - k + 1];
        //queue 1 3 -1 
        // j = 0, i = -2 -1 
        for(int j = 0, i = 1 - k; j < nums.length; i++, j++) {
            if(i > 0 && deque.peekFirst() == nums[i - 1])
                deque.removeFirst(); // 删除 deque 中对应的 nums[i-1]
            while(!deque.isEmpty() && deque.peekLast() < nums[j])
                deque.removeLast(); // 保持 deque 递减
            deque.addLast(nums[j]);
            if(i >= 0)
                res[i] = deque.peekFirst();  // 记录窗口最大值
        }
        return res;
    }		
```

## 502. IPO

> #### [502. IPO](https://leetcode-cn.com/problems/ipo/)
>
> 难度困难39
>
> 假设 力扣（LeetCode）即将开始其 IPO。为了以更高的价格将股票卖给风险投资公司，力扣 希望在 IPO 之前开展一些项目以增加其资本。 由于资源有限，它只能在 IPO 之前完成最多 **k** 个不同的项目。帮助 力扣 设计完成最多 **k** 个不同项目后得到最大总资本的方式。
>
> 给定若干个项目。对于每个项目 **i**，它都有一个纯利润 **Pi**，并且需要最小的资本 **Ci** 来启动相应的项目。最初，你有 **W** 资本。当你完成一个项目时，你将获得纯利润，且利润将被添加到你的总资本中。
>
> 总而言之，从给定项目中选择最多 **k** 个不同项目的列表，以最大化最终资本，并输出最终可获得的最多资本。
>
> **示例 1:**
>
> ```
> 输入: k=2, W=0, Profits=[1,2,3], Capital=[0,1,1].
> 
> 输出: 4
> 
> 解释:
> 由于你的初始资本为 0，你尽可以从 0 号项目开始。
> 在完成后，你将获得 1 的利润，你的总资本将变为 1。
> 此时你可以选择开始 1 号或 2 号项目。
> 由于你最多可以选择两个项目，所以你需要完成 2 号项目以获得最大的资本。
> 因此，输出最后最大化的资本，为 0 + 1 + 3 = 4。
> ```

### 优先队列

实现思路

定义一个节点 p为利润 c为成本。实现两个比较器，最小成本比较器。最大利润比较器。

首先定义node数组，将成本和利润存储到数组中。遍历将成本存储到小顶堆中。

每次从小顶堆中拿到最小的成本。

遍历k，因为最多k次交易，当小顶堆中不为null并且当前的小顶堆元素小于等于当前以有成本，就进行交易。

大顶堆中存储当前交易的最大利润订单。w+=最大订单利润。

终止结束条件，当大顶堆为null  或者 交易次数达到k次，结束。

```java
	private static class Node{
       int p;//利润
       int c;//成本

       public Node(int p,int c){
           this.p = p;
           this.c = c;
       }
   }

   //最小的成本
   public class MinP implements Comparator<Node>{

       @Override
       public int compare(Node o1, Node o2) {
           return o1.c - o2.c;
       }
   }

   //最大的利润
   public class MaxC implements Comparator<Node>{
       @Override
       public int compare(Node o1, Node o2) {
           return o2.p - o1.p;
       }
   }

    public int findMaximizedCapital(int k, int W, int[] Profits, int[] Capital) {
        Node [] nodes  = new Node[Profits.length];
        for (int i = 0; i < nodes.length; i++) {
            nodes[i] = new Node(Profits[i],Capital[i]);
        }

        PriorityQueue<Node> minPQ = new PriorityQueue<>(new MinP());
        PriorityQueue<Node> maxPQ = new PriorityQueue<>(new MaxC());

        for (int i = 0; i < nodes.length; i++) {
            minPQ.add(nodes[i]);
        }
        for (int i = 0; i < k; i++) {
            while (!minPQ.isEmpty() || minPQ.peek().c <= W) {
                maxPQ.add(minPQ.poll());
            }
            if (maxPQ.isEmpty()) {
                return W;
            }
            W+=maxPQ.poll().p;
        }
        return W;
    }
```



## [199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)

> 难度中等371
>
> 给定一棵二叉树，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。
>
> **示例:**
>
> ```
> 输入: [1,2,3,null,5,null,4]
> 输出: [1, 3, 4]
> 解释:
> 
>    1            <---
>  /   \
> 2     3         <---
>  \     \
>   5     4       <---
> ```

```java
	//整体的思路的话
    //我们直接用层序遍历，拿到最后的一个数 直接添加就可以了
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> ret = new ArrayList<>();
        if (root == null) return ret;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i =0 ; i < size ; i++) {
                TreeNode cur = queue.poll();
                if (cur == null) continue;
                if (cur.left != null) {
                    queue.add(cur.left);
                } 
                if (cur.right != null) {
                    queue.add(cur.right);
                }
                if (size - i == 1) {
                    ret.add(cur.val);
                }
            }
        }
        return ret;
    }
```

