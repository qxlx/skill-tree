

# 栈

## 20.有效的括号  ☆ 

> 难度简单1475
>
> 给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串，判断字符串是否有效。
>
> 有效字符串需满足：
>
> 1. 左括号必须用相同类型的右括号闭合。
> 2. 左括号必须以正确的顺序闭合。
>
> 注意空字符串可被认为是有效字符串。
>
> **示例 1:**
>
> ```
> 输入: "()"
> 输出: true
> ```

```java
//使用栈   
public boolean isValid(String s) {
        if(s.length() == 0){
            return true;
        }
        Stack<Character> stack = new Stack();
        for(char c : s.toCharArray()){
            //如果是 [ { ( push到Stack中
            if(c == '{' || c == '[' || c == '('){
                stack.push(c);
            }else{
                if(stack.isEmpty()){
                    return false;
                }
                //将Stack中 [  { ( pop出来 和 c中的遍历 如果相等就消去一对
                Character ch = stack.pop();
                boolean a = (c == '}' && ch != '{');
                boolean b = (c == ']' && ch != '[');
                boolean d = (c == ')' && ch != '(');
                if(a || b || d){
                    return false;
                }
            }
        }
        //最后如果栈中为null 说明 符号正确
        return stack.isEmpty();
    }
```

时间复杂度:O(n)



时间复杂度:O(n^2/2)

```java
public boolean isValid(String s) {
        int length;
        do{
            length = s.length();
            s = s.replace("()","").replace("[]","").replace("{}","");
        }while(length != s.length());
        return s.isBlank();
    }
```

## 155.最小栈 ☆

> 设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。
>
> push(x) -- 将元素 x 推入栈中。
> pop() -- 删除栈顶的元素。
> top() -- 获取栈顶元素。
> getMin() -- 检索栈中的最小元素。
> 示例:
>
> MinStack minStack = new MinStack();
> minStack.push(-2);
> minStack.push(0);
> minStack.push(-3);
> minStack.getMin();   --> 返回 -3.
> minStack.pop();
> minStack.top();      --> 返回 0.
> minStack.getMin();   --> 返回 -2.

time:O(1)

space:O(n)

th：最小栈的实现 一般通过两个栈来实现 一个数据栈 一个存储最小值的栈  在push和pop的时候 注意 push的时候 数据栈直接存储  但是对于minStack来说 需要比较得出最先栈的值  而pop的时候 需要进行判断是否会存在如果minStack为空的话需要 将Integer.max_value 存储 

```java
 /** initialize your data structure here. */
    Stack<Integer> dataStack;//数据栈
    Stack<Integer> minStack;//最小值栈

    int min = 0;

    public MinStack() {
        dataStack = new Stack();
        minStack = new Stack();
        min = Integer.MAX_VALUE;//存储一个最大值 用作比较
    }
    
    public void push(int x) {
        dataStack.push(x);
        min = Math.min(min,x);
        minStack.add(min);
    }
    
    public void pop() {
        dataStack.pop();
        minStack.pop();
        min = minStack.isEmpty() ? Integer.MAX_VALUE : minStack.peek();
    }
    
    public int top() {
        return dataStack.peek();
    }
    
    public int getMin() {
        return min;
    }
```

## 84.柱状图中最大的矩形 ☆ 

> #### [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)
>
> 难度困难509收藏分享切换为英文关注反馈
>
> 给定 *n* 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。
>
> 求在该柱状图中，能够勾勒出来的矩形的最大面积。
>
>  
>
> ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/histogram.png)
>
> 以上是柱状图的示例，其中每个柱子的宽度为 1，给定的高度为 `[2,1,5,6,2,3]`。
>
>  
>
> ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/histogram_area.png)
>
> 图中阴影部分为所能勾勒出的最大矩形面积，其面积为 `10` 个单位。
>
>  
>
> **示例:**
>
> ```
> 输入: [2,1,5,6,2,3]
> 输出: 10
> ```

### 1.暴力破解

思路

两层遍历确定每一层最大的面积，第三层循环确定每次最底的高， j-i+1 长 * 高 求出每次的面积

time:O(n^3)

space:O(1)

```java
  public int largestRectangleArea(int[] heights) {
        int maxArea = 0;
        for(int i=0;i<heights.length;i++){ //多少轮
            for(int j=i;j<heights.length;j++){ //多少次
                int minHeigh = Integer.MAX_VALUE;
                for(int k=i;k<=j;k++)//每一次中最小值
                    minHeigh = Math.min(minHeigh,heights[k]);

                maxArea = Math.max(maxArea,minHeigh*(j-i+1));//每一次计算出最大值
            }
        }
        return maxArea;
    }
```

### 2.优化暴力

思路：每次利用之前的最低高

time:O(n^2)

space:O(1)

```java
public int largestRectangleArea(int[] heights) {
        int maxArea = 0;
        for(int i=0;i<heights.length;i++){ //多少轮
            int minHeigh = Integer.MAX_VALUE;
            for(int j=i;j<heights.length;j++){ //多少次
                minHeigh = Math.min(minHeigh,heights[j]);
                maxArea = Math.max(maxArea,minHeigh*(j-i+1));//每一次计算出最大值
            }
        }
        return maxArea;
    }
```

### 3.栈



time:O(n)

space:O(n)

```java
 public int largestRectangleArea(int[] heights) {
        Stack < Integer > stack = new Stack < > ();
        stack.push(-1);
        int maxarea = 0;
        for (int i = 0; i < heights.length; ++i) {
            while (stack.peek() != -1 && heights[stack.peek()] >= heights[i])
                maxarea = Math.max(maxarea, heights[stack.pop()] * (i - stack.peek() - 1));
            stack.push(i);
        }
        while (stack.peek() != -1)
            maxarea = Math.max(maxarea, heights[stack.pop()] * (heights.length - stack.peek() -1));
        return maxarea;
    }
```





## 栈实现队列

```java
/**使用两个栈（一个压入栈，一个弹出栈）实现一个队列。要注意在弹出元素栈的地方要注意栈的变化，每次压入栈中的元素移动到弹出栈时，要全部移动。当弹出栈中有元素的时候，不能从压入栈中移动元素到弹出栈中。
    */
    private Stack<Integer> in;  //进栈
    private Stack<Integer> out; //弹出栈
    /** Initialize your data structure here.*/
    public MyQueue() {
        in = new Stack<>();
        out = new Stack<>();
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        in.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        if(out.isEmpty()){
            int size = in.size();
            for(int i=0;i<size;i++){
                out.push(in.pop());
            }
        }
        return out.pop();
    }
    
    /** Get the front element. */
    public int peek() {
        if(out.isEmpty()){
            int size = in.size();
            for(int i=0;i<size;i++){
                out.push(in.pop());
            }
        }
        return out.peek();
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return in.isEmpty() && out.isEmpty();
    }
```

## 队列实现栈

```java
/** Initialize your data structure here. */
    private Queue<Integer> q;
    private Integer tail;//记录栈顶元素

    public MyStack() {
        q = new LinkedList<>();
        tail = 0;
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        q.add(x);
        tail = x;//每添加一个元素 就将最后一个元素赋值给tail
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        for(int i=0;i<q.size()-2;i++){
            q.add(q.remove());
        }
        tail = q.remove();//将倒数第二个元素存储到tail 用以弹出tail
        q.add(tail);
        return q.remove();
    }
    
    /** Get the top element. */
    public int top() {
        return tail;
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return q.isEmpty();
    }
```

最精妙之处，队列是一个先进先出的结构，而我们每次添加的时候。会将当前数据进行前后操作。比如我们添加了一个数据1,2 如果是栈的话 结构从顶到下 就是2,1 。 而队列是如何操作的，将当前进行前后操作，将第一个元素 添加到队列后，至于为什么终止条件为1 因为当剩余一个的时候，就一定是当前操作刚添加进来的数据。至在了前面。

```java
	private Queue<Integer> queue;

    /** Initialize your data structure here. */
    public MyStack() {
        queue = new LinkedList<>();
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        queue.add(x);
        int size = queue.size();
        while (size-- > 1) {
            queue.add(queue.poll());
        }
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return queue.remove();
    }
    
    /** Get the top element. */
    public int top() {
        return queue.peek();
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return queue.size() == 0;
    }
```

## [739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

> 难度中等564
>
> 请根据每日 `气温` 列表，重新生成一个列表。对应位置的输出为：要想观测到更高的气温，至少需要等待的天数。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。
>
> 例如，给定一个列表 `temperatures = [73, 74, 75, 71, 69, 72, 76, 73]`，你的输出应该是 `[1, 1, 4, 2, 1, 1, 0, 0]`。
>
> **提示：**`气温` 列表长度的范围是 `[1, 30000]`。每个气温的值的均为华氏度，都是在 `[30, 100]` 范围内的整数。

```java
 	//单调栈
    //总体思路是将小于当前栈顶的元素添加入栈，那么在弹出栈的过程中
    //只要有大于当前元素的值一定可以找到比当前数大于的数，而栈中记录的是
    //下标值
    public int[] dailyTemperatures(int[] T) {
        if (T == null) return new int [0];
        int [] ret = new int [T.length];

        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < T.length; i++) {
            int temperature = T[i];
            while (!stack.isEmpty() && temperature > T[stack.peek()]) {
                int preIndex = stack.pop();
                ret[preIndex] = i - preIndex;
            }
            stack.push(i);
        }
        return ret;
    }
```



## [503. 下一个更大元素 II](https://leetcode-cn.com/problems/next-greater-element-ii/)

> 难度中等217
>
> 给定一个循环数组（最后一个元素的下一个元素是数组的第一个元素），输出每个元素的下一个更大元素。数字 x 的下一个更大的元素是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出 -1。
>
> **示例 1:**
>
> ```
> 输入: [1,2,1]
> 输出: [2,-1,2]
> 解释: 第一个 1 的下一个更大的数是 2；
> 数字 2 找不到下一个更大的数； 
> 第二个 1 的下一个最大的数需要循环搜索，结果也是 2。
> ```

```java
	public int[] nextGreaterElements(int[] nums) {
        int n = nums.length;
        if (nums == null || n == 0) return new int[0];
        int [] ret = new int [n];
        Arrays.fill(ret,-1);
        Stack<Integer> stack  = new Stack<>();
        for (int i=0; i < 2*n; i++) {
            int num = nums[i % n];
            while (!stack.isEmpty() && num > nums[stack.peek()]) {
                ret[stack.pop()] = num;
            }
            if ( i < n) {
                stack.push(i);
            }
        }
        return ret;
    }
```

