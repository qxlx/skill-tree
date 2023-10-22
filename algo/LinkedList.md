# LinkedList

高频题

## [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)？？

> 给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
>
> 示例:
>
> 输入: [-2,1,-3,4,-1,2,1,-5,4],
> 输出: 6
> 解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
> 进阶:
>
> 如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。
>



```java
public int crossSum(int[] nums, int left, int right, int p) {
        if (left == right) return nums[left];

        int leftSubsum = Integer.MIN_VALUE;
        int currSum = 0;
        for (int i = p; i > left - 1; --i) {
            currSum += nums[i];
            leftSubsum = Math.max(leftSubsum, currSum);
        }

        int rightSubsum = Integer.MIN_VALUE;
        currSum = 0;
        for (int i = p + 1; i < right + 1; ++i) {
            currSum += nums[i];
            rightSubsum = Math.max(rightSubsum, currSum);
        }

        return leftSubsum + rightSubsum;
    }

    public int helper(int[] nums, int left, int right) {
        if (left == right) return nums[left];

        int p = (left + right) / 2;

        int leftSum = helper(nums, left, p);
        int rightSum = helper(nums, p + 1, right);
        int crossSum = crossSum(nums, left, right, p);

        return Math.max(Math.max(leftSum, rightSum), crossSum);
    }

    public int maxSubArray(int[] nums) {
        return helper(nums, 0, nums.length - 1);
    }
```


## 141.环形链表  ⭐⭐⭐

> 给定一个链表，判断链表中是否有环。
>
> 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。
>
> 示例 1：
>
> 输入：head = [3,2,0,-4], pos = 1
> 输出：true
> 解释：链表中有一个环，其尾部连接到第二个节点。

### 1.快慢指针	

可以类比成两个运动员 在同一个环形运动场跑步，一个跑的快的 和 一个跑的慢的 在一定的时间内 快的一定会和慢的相遇，这个时候 就说明 有环存在。

```java
	//检查是否链表中有环
    //思路:定义一个快指针 和 一个慢指针 慢指针一次走一步 快指针一次走两步
    //如果有环 一定会相遇。多次循环
    public boolean hasCycle(ListNode head) {
        if (head == null){
            return false;
        }
        ListNode fastNode = head.next;
        ListNode slowNode = head;
        while (fastNode!=null && fastNode.next!=null){
            fastNode = fastNode.next.next;
            slowNode = slowNode.next;
            if (slowNode == fastNode)
                return true;
        }
        return false;
    }

```

分析:

时间复杂度:o(n)

1.第一种情况  链表不存在环 快慢指针分别到达尾部，其时间取决于列表的长度。O(n)

2.第二种情况  链表中存在环  在最糟糕的情形下，时间复杂度为 O(N+K)O(N+K)，也就是 O(n)

空间复杂度 o(1)

只是用了快慢指针两个结点。

### 2.哈希表

思路: 通过创建哈希表，遍历链表 将每次遍历的都存储到哈希表中 如果哈希表中有当前哈希值 说明 遍历到了环形处，说明是环形链表 否则的话 遍历完  说明不是环形链表

```java
public boolean hasCycle2(ListNode head) {
        Set nodeSet = new HashSet();
        while (head!=null){
            if (nodeSet.contains(head)){
                return true;
            }
            nodeSet.add(head);
            head = head.next;
        }
        return false;
    }
```

时间复杂度：o(n) 对于含有n个元素的链表 

空间复杂度:   o(n) 最坏情况下 全部添加 所有取决于哈希表中的元素数目

## 23.合并K个排序链表

> 合并 k 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。
>
> 示例:
>
> 输入:
> [
>   1->4->5,
>   1->3->4,
>   2->6
> ]
> 输出: 1->1->2->3->4->4->5->6

### 1.不使用递归

基本思路：我们可以使用分治思想 来解决这个问题。当一个链表集合中需要合并并排序。我们可以假设只有2个 每次合并两个。两个合并一个。这样就可以获取到最终的结果。

第一次将list[0]与list[len-1]合并 2次list[1]与list[len-2] 经过 for循环一次后。可以等到一半的list链表结合。在依次len = len/2; 依次合并 就可以获取到下标为0的为头结点的这个链表。

```java
//合并两个有序的链表
    public ListNode mergeKLists(ListNode[] lists) {
        int len = lists.length;
        if (len == 0)
            return null;
        while (len > 1) {
            for (int i = 0; i < len / 2; i++) {
                lists[i] = merageKList(lists[i],lists[len-1-i]);
            }
            len = (len+1)/2;
        }
        return lists[0];
    }

    //合并两个有序的链表 
    private ListNode merageKList(ListNode l1, ListNode l2) {
        if (l1 == null)
            return null;
        if (l2 == null)
            return null;

        ListNode listNode = new ListNode(-1);
        ListNode head = listNode;

        while (l1 != null && l2 != null) {
            if (l1.data < l2.data) {
                head.next = l1;
                l1 = l1.next;
            } else {
                head.next = l2;
                l2 = l2.next;
            }
            head = head.next;
        }

        if (l1 != null){
            head.next = l1;
        }else{
            head.next = l2;
        }
        return listNode.next; //note 注意返回的不是head 返回head 会带有 -1这个结点的值 只需要后边的值 牛掰思想
    }
```

时间复杂度：整体时间复杂度为O(N*log(k)), k为链表个数，N为链表平均长度。

## 24.两两交换链表中的节点 ☆

```
//给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。 
//
// 你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。 
//
// 
//
// 示例: 
//
// 给定 1->2->3->4, 你应该返回 2->1->4->3.
// 
// Related Topics 链表
```

### 1.递归

thinking: 使用递归的方法  每一次递归都交换一对节点，用firstNode.next 记录上一次节点交换后的首节点。secondNode节点作为首节点。

```java
public ListNode swapPairs(ListNode head) {
        //if the list has no node or has only one node left
        if(head == null || head.next == null){
            return head;
        }

        //Nodes to be swapped
        ListNode firstNode = head;
        ListNode secondNode = head.next;

        //swapping
        firstNode.next = swapPairs(secondNode.next);
        secondNode.next = firstNode;

        //Now the head is the second node
        return secondNode;
    }
```

- 时间复杂度：O(N)    O(N)，其中 N 指的是链表的节点数量。
- 空间复杂度：O(N)    O(N)，递归过程使用的堆栈空间。

#### -递归简洁版

```java
 public ListNode swapPairs(ListNode head) {
        if ((head == null)||(head.next == null))
            return head;
        ListNode n = head.next;
        head.next = swapPairs(head.next.next);
        n.next = head;
        return n;
    }
```

### 2.迭代法

thinking：定义一个前驱节点，记录每次交换后的节点变化

head节点和pre节点每次交换完成后重置。

```java
/***
     *  迭代法
     *  1.定义一个前驱节点，用以记录每次交换后的元素的前置节点
     *  2.交换
     *  3.head 节点 和pre节点重置
     * @param head
     * @return
     */
    public ListNode swapPairs(ListNode head) {

        ListNode dummy = new ListNode(-1);
        dummy.next = head;

        ListNode pre = dummy;

        while ((head!=null) && (head.next !=null)){

            ListNode firstNode = head;
            ListNode secondNode = head.next;

            //swap
            pre.next = secondNode;//-1 -> 2
            firstNode.next = secondNode.next;// 1 -> 3
            secondNode.next = firstNode;

            pre = firstNode;
            head = firstNode.next;
        }
        return dummy.next;
    }
```

- 时间复杂度：O(N)，其中 N 指的是链表的节点数量。
- 空间复杂度：O(1)

```java
	public ListNode swapPairs(ListNode head) {
        ListNode node = new ListNode(-1);
        node.next = head;
        //不是传递head的原因在于 如果直接获取head 那么交换的就是下2,3节点了
        ListNode pre = node;

        while (pre.next != null && pre.next.next != null) {
            //存储当前节点 和下一个节点
            ListNode l1 = pre.next ,l2 = pre.next.next;
            //存储当前非交换节点的下一个节点
            ListNode next = l2.next;

            //交换
            //l2节点和pre连接
            pre.next = l2;
            //l1在l2之后 连接上next链表
            l1.next = next;
            //l2 连接上l1
            l2.next = l1;
            
            //将链表指向pre  l1->l2 交换后为 l2->l1 所以 下一个就是交换的
            pre = l1;
        }
        return node.next;
    }
```



## 25.K 个一组翻转链表

> 难度困难416
>
> 给你一个链表，每 *k *个节点一组进行翻转，请你返回翻转后的链表。
>
> *k *是一个正整数，它的值小于或等于链表的长度。
>
> 如果节点总数不是 *k *的整数倍，那么请将最后剩余的节点保持原有顺序。
>
>  
>
> **示例：**
>
> 给你这个链表：`1->2->3->4->5`
>
> 当 *k *= 2 时，应当返回: `2->1->4->3->5`
>
> 当 *k *= 3 时，应当返回: `3->2->1->4->5`
>
>  
>
> **说明：**
>
> - 你的算法只能使用常数的额外空间。
> - **你不能只是单纯的改变节点内部的值**，而是需要实际进行节点交换。



```java
/***
     * 迭代法
     * 1.链表分区已翻转 + 待翻转 + 未翻转部分
     * 2.翻转前确定翻转的范围 通过k来决定
     * 3.记录链表前驱和后继 方便翻转完成后，把已翻转和未翻转连接起来。
     * 4.初始化两个边路pre  end  pre >代表待翻转链表的前驱，end代表待翻转的末尾。
     * 5.经过k次 end到达链表末尾。  记录待翻转链表的后继 next = end.next
     * 6.翻转链表，将三部分连接起来，然后重置pre 和 end 指针 进入下一个循环
     * 7.特殊情况 翻转部分长度不足k时，在定位end 完成后，end = null 已经到达末尾。
     * 8.时间复杂度为 O(n*K) 最好的情况为 O(n) 最差的情况未 O(n^2)
     * 9.空间复杂度为 O(1)
     * @param head
     * @param k
     * @return
     */
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode dumy = new ListNode(0);
        dumy.next = head;

        ListNode pre = dumy;
        ListNode end = dumy;

        while (end.next != null) {
            //确定待翻转的范围
            for (int i = 0; i < k && end != null; i++) {
                end = end.next;
            }

            if (end == null) {
                break;
            }

            ListNode start = pre.next;//待翻转链表的开始位置
            ListNode next = end.next;//下一个待翻转链表的起始位置

            end.next = null;//和后继待翻转链表断开

            pre.next = reverse(start);
            start.next = next;
            pre = start;

            end = pre;
        }
        return dumy.next;
    }

    public static ListNode reverse(ListNode head) {
        ListNode pre = null;
        ListNode curr = head;
        //假设 1 -> 2 -> 3
        while (curr != null) {
            ListNode next = curr.next; // next = 2   // next = 3
            curr.next = pre;// 1.next = null        // 3.next = 1
            pre = curr;// pre = 1;                 //  1 = 3
            curr = next; // curr = 2              // 3  = null
        }
        return pre; //
    }
```

## 21. 合并两个有序链表

> 将两个升序链表合并为一个新的升序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 
>
> **示例：**
>
> ```
> 输入：1->2->4, 1->3->4
> 输出：1->1->2->3->4->4
> ```
>
> 

### 1.迭代

th:主要是通过定义一个head节点 遍历两个链表就可以，最后一定会有一个链表没有遍历完 使用三目运算符进行算

**需要注意的点**  因为head节点一直next next下去 所以应该定义一个root节点 作为根节点 head节点 一直next下去。返回的是root节点

time:O(m+n)

space:O(1) 

```java
/*
    *思路 主要是通过定义一个head节点 遍历两个链表就可以，最后一定会有一个链表没有遍历完 使用三目运算符进行算
    notice 因为head节点一直next next下去 所以应该定义一个root节点 作为根节点 head节点 一直next下去。返回的是root节点
    */
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null || l2 == null){
            return null;
        }

        ListNode root = new ListNode(-1);

        ListNode head = root;

        while(l1!=null && l2!= null){
            if(l1.val <= l2.val){
                head.next = l1;
                l1 = l1.next; 
            }else{
                head.next = l2;
                l2 = l2.next;
            }
            head = head.next;
        }

        head.next = (l1 == null ? l2 : l1);

        return root.next;
    }
```

### 2.递归

th：其实如果一个方法可以迭代就可以递归解决，但是递归代码虽然简洁 理解上比较难一些，递归代码无非就是 三点需要注意：1 终止条件 2. 业务逻辑 3.解决下一个子问题 也就是 调用自身。

time:O(m+n)

space:调用 mergeTwoLists 退出时 l1 和 l2 中每个元素都一定已经被遍历过了，所以 n + mn+m 个栈帧会消耗 O(n + m)O(n+m) 的空间。

```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
       if(l1 == null ){
           return l2;
       }else if(l2 ==  null){
           return l1;
       }else if(l1.val < l2.val){
           l1.next = mergeTwoLists(l1.next,l2);
           return l1;
       }else {
           l2.next = mergeTwoLists(l1,l2.next);
           return l2;
       }
    }
```

## 2.两数相加

> #### [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)
>
> 难度中等4562
>
> 给出两个 **非空** 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 **逆序** 的方式存储的，并且它们的每个节点只能存储 **一位** 数字。
>
> 如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。
>
> 您可以假设除了数字 0 之外，这两个数都不会以 0 开头。
>
> **示例：**
>
> ```
> 输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
> 输出：7 -> 0 -> 8
> 原因：342 + 465 = 807
> ```

时间复杂度为O(Math.max(m,n))

```java
/**
        思路为 定义一个pre指针指向整个链表，终止条件为任意一个节点为null的时候退出。也可以计算出时间复杂度为Math.max(m,n).
        其中 m n分别指代l1 l2指针的长度、这块需要注意的就是如果有进位的话 我们必须记录进位。获取一个位的进位 sum/10
        获取余数为sum%10;另外如果在最后进位为1 需要创建一个新节点存储新的进位值。
    */
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode pre = new ListNode(-1);
        ListNode cur = pre;
        int carry = 0;
        while(l1!=null || l2 !=null){
            int x = l1 == null ? 0 : l1.val;
            int y = l2 == null ? 0 : l2.val;
            int sum = x+y+carry;

            carry = sum/10;
            sum = sum%10;
            cur.next = new ListNode(sum);
            cur = cur.next;

            if(l1!=null){
                //区分 if(l1!=null) 和 if(l1.next!=null) 
                //前者l1为null的时候
                l1 = l1.next;
            }

            if(l2!=null){
                l2 = l2.next;
            }
        }

        if(carry==1){
            cur.next = new ListNode(carry);
        }
        return pre.next;
    }
```

## [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

> #### [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)
>
> 难度中等885
>
> 给定一个链表，删除链表的倒数第 *n* 个节点，并且返回链表的头结点。
>
> **示例：**
>
> ```
> 给定一个链表: 1->2->3->4->5, 和 n = 2.
> 
> 当删除了倒数第二个节点后，链表变为 1->2->3->5.
> ```



```java
	//快慢指针，这块删除不难，主要是要处理好 1->2 删除1这样的问题。
    //所以需要设定一个前置指针，至于为什么fast 和 slow要指向pre
    //1>2>3>4>5 fast指向1的话 先走2步 到达3位置。
    //slow指向1 和 fast并行走 fast到5的位置走不动  slow走到3 然后删除4 没有问题。
    //但是当删除头结点 1 就没有办法了。必须有一个前置指针指向1 所以是需要添加一个前置指针的Pre。
    //时间复杂度为O(n) 空间复杂度O(1)
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode pre = new ListNode(0);
        pre.next = head;
        ListNode fast = pre, slow = pre;
        
        while(n!=0){
            fast = fast.next;
            n--;
        }

        while(fast.next!=null){
            fast = fast.next;
            slow = slow.next;
        }
        slow.next = slow.next.next;
        return pre.next;
    }
```

```java
 	//快慢指针+先走k步
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if (head == null) return head;
        ListNode fast = head;

        while (n-- > 0) {
            fast = fast.next;
        }

        //判断是否为null
        //ps : 返回head.next是比较牛
        if (fast == null) return head.next;

        ListNode slow = head;
        while (fast.next !=null) {
            slow = slow.next;
            fast = fast.next;
        }
        
        if (slow.next != null) {
            slow.next = slow.next.next;
        }
        return head;
    }
```



## [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

> #### [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)
>
> 难度中等533
>
> 给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 `null`。
>
> 为了表示给定链表中的环，我们使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 `pos` 是 `-1`，则在该链表中没有环。
>
> **说明：**不允许修改给定的链表。
>
>  
>
> **示例 1：**
>
> ```
> 输入：head = [3,2,0,-4], pos = 1
> 输出：tail connects to node index 1
> 解释：链表中有一个环，其尾部连接到第二个节点。
> ```
>
> ![img](e:\pic\circularlinkedlist.png)



```java
//  快慢指针，fast和slow同时走。
//  fast每次走2步。而slow每次走1步。 从head到环入口为a  b为环的圈数，而k为多少次。
//  1.第一个公式fast每次比slow多走1步 关系为f = 2s  
//  2.f=s+nb fast和slow都走过a步  fast比slow多走n圈。  所有s = nb
//  3.走入环的步数为 k= a+nb  所以k-a = s; k = a+s
//  4.时间复杂度为O(n) 
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head, slow = head;
        while(true){
            if(fast == null || fast.next == null){
                return null;
            }
            slow = slow.next;
            fast = fast.next.next;
            if(fast == slow){
                break;
            }
        }

        fast = head;
        while(fast!=slow){
            fast = fast.next;
            slow = slow.next;
        }
        return slow;
    }
}
```

## [237. 删除链表中的节点](https://leetcode-cn.com/problems/delete-node-in-a-linked-list/)

> #### [237. 删除链表中的节点](https://leetcode-cn.com/problems/delete-node-in-a-linked-list/)
>
> 难度简单720
>
> 请编写一个函数，使其可以删除某个链表中给定的（非末尾）节点，你将只被给定要求被删除的节点。
>
> 现有一个链表 -- head = [4,5,1,9]，它可以表示为:
>
> ![img](e:\pic\237_example.png)
>
>  
>
> **示例 1:**
>
> ```
> 输入: head = [4,5,1,9], node = 5
> 输出: [4,1,9]
> 解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.
> ```



```java
	//通常我们删除节点都是通过将要删除节点的前一个指针和呆删除节点的后一个指针  连接。
    // 但是此题明显不能，只给了删除的节点，所以 我们可以将要删除节点的val修改称下一个节点的val。然后删除当前节点.next
    // 时间复杂度O(1)
    public void deleteNode(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
    }
```

## 牛客.重复节点删除

> 重复节点删除

```java
public ListNode deleteDuplication(ListNode pHead)
    {
        if(pHead == null){
            return null;
        }
 
        //设置前置指针
        ListNode head = new ListNode(-1);
        head.next = pHead;
        ListNode pre = head;
        ListNode cur = head.next;
 
        //判断是否为null
        while(cur!=null){
            if(cur.next!=null && cur.val == cur.next.val){
                while(cur.next!=null && cur.val == cur.next.val){
                    cur = cur.next;
                }
                pre.next = cur.next;
                if(cur.next==null){
                    return head.next;
                }
                cur = cur.next;
            }else {
                pre = cur;
                cur = cur.next;
            }
        }
        return head.next;
    }
```

## [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

> 难度简单420
>
> 给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。
>
> **示例 1:**
>
> ```
> 输入: 1->1->2
> 输出: 1->2
> ```
>
> **示例 2:**
>
> ```
> 输入: 1->1->2->3->3
> 输出: 1->2->3
> ```

```java
	//如果head==null 或者  head.next == null 直接返回head;
    //只考虑当前层问题 
    //如果当前节点和下一个节点的val相等 返回下一个节点 否则 返回当前节点
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null || head.next == null) return head;
        head.next = deleteDuplicates(head.next);
        return head.val == head.next.val ? head.next : head;
    }
```

## [445. 两数相加 II](https://leetcode-cn.com/problems/add-two-numbers-ii/)

> 难度中等296
>
> 给你两个 **非空** 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。
>
> 你可以假设除了数字 0 之外，这两个数字都不会以零开头。
>
>  
>
> **进阶：**
>
> 如果输入链表不能修改该如何处理？换句话说，你不能对列表中的节点进行翻转。
>
>  
>
> **示例：**
>
> ```
> 输入：(7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
> 输出：7 -> 8 -> 0 -> 7
> ```

```java
	//主要思想就是通过两个栈 将数据弹出 分别计算出对应位的和 
    //利用链表的头插法 将整个链表进行连接上
    //时间复杂度O(n)
    //空间复杂度O(N) 
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        //if (l1 == null || l2 == null) return null;
        Stack<Integer> s1 = buildStack(l1);
        Stack<Integer> s2 = buildStack(l2);

        ListNode head = new ListNode(-1);
        int carry = 0;
        while (!s1.isEmpty() || !s2.isEmpty() || carry != 0) {
            int num1 = s1.isEmpty() ? 0 : s1.pop();
            int num2 = s2.isEmpty() ? 0 : s2.pop();
            int sum = num1 + num2 + carry;
            ListNode node = new ListNode(sum % 10);
            //头插法
            node.next = head.next;//将当前head之后的链表 和 node.next连接上
            head.next = node;//将node添加到head之后
            //如果当前进位为true 
            //不能判断为 sum/10 因为 当下次不满足10时，carry=1就会一直保存
            //当while条件一直符合时，就一直循环 称为一个死循环
            carry = sum / 10;
        }
        return head.next;
    }

    private Stack<Integer> buildStack(ListNode l1) {
        Stack <Integer> s1 = new Stack<>();
        while (l1 != null) {
            s1.push (l1.val);
            l1 = l1.next;
        }
        return s1;
    }
```



## [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

> 难度简单756
>
> 请判断一个链表是否为回文链表。
>
> **示例 1:**
>
> ```
> 输入: 1->2
> 输出: false
> ```
>
> **示例 2:**
>
> ```
> 输入: 1->2->2->1
> 输出: true
> ```

```java
	public boolean isPalindrome(ListNode head) {
        if (head == null) return true;
        Stack<Integer> stack = new Stack<>();
        ListNode cur = head;
        while (cur != null) {
            stack.push(cur.val);
            cur = cur.next;
        }

        //判断两个
        while (!stack.isEmpty()) {
            int val = stack.pop();
            if (head.val != val) {
                return false;
            }
            head = head.next;
        }
        return true;
    }
```

## [328. 奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)

> 难度中等267
>
> 给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。
>
> 请尝试使用原地算法完成。你的算法的空间复杂度应为 O(1)，时间复杂度应为 O(nodes)，nodes 为节点总数。
>
> **示例 1:**
>
> ```
> 输入: 1->2->3->4->5->NULL
> 输出: 1->3->5->2->4->NULL
> ```

```java
	//用两个指针 分别指向偶数和奇数 
    // head 是头结点 当遍历完毕时 将奇数的末尾和偶数的头部连接上
    // 时间复杂度O(n) 空间复杂度为O(1)
    public ListNode oddEvenList(ListNode head) {
       if (head == null) return null;
       ListNode odd = head, event = head.next, eventHead = event;

       while (event != null && event.next != null) {
           odd.next = odd.next.next;
           odd = odd.next;
           event.next = event.next.next;
           event = event.next;
       } 
       odd.next = eventHead;
        return head;
    }
```





## [725. 分隔链表](https://leetcode-cn.com/problems/split-linked-list-in-parts/)

> 难度中等104
>
> 给定一个头结点为 `root` 的链表, 编写一个函数以将链表分隔为 `k` 个连续的部分。
>
> 每部分的长度应该尽可能的相等: 任意两部分的长度差距不能超过 1，也就是说可能有些部分为 null。
>
> 这k个部分应该按照在链表中出现的顺序进行输出，并且排在前面的部分的长度应该大于或等于后面的长度。
>
> 返回一个符合上述规则的链表的列表。
>
> 举例： 1->2->3->4, k = 5 // 5 结果 [ [1], [2], [3], [4], null ]

```java
	public ListNode[] splitListToParts(ListNode root, int k) {
        if (root == null) return new ListNode[k];
        ListNode [] ret = new ListNode[k];
        int sum = 0;
        ListNode cur = root;

        while (cur != null) {
            sum++;
            cur = cur.next;
        }            

        int size = sum / k;
        int mod = sum % k;

        cur = root;

        for (int i = 0; i < k && cur != null; i++) {
            ret[i] = cur;
            int curSize = size + (mod-- > 0 ? 1 : 0);
            //终止条件curSize-1 是因为之前已经遍历了一个元素
            for (int j = 0 ; j < curSize - 1; j++ ) {
                cur = cur.next;
            }
            ListNode next = cur.next;
            cur.next = null;
            cur = next;
        }
        return ret;
    }
```



## [1669. 合并两个链表](https://leetcode-cn.com/problems/merge-in-between-linked-lists/)

```java
	//因为我们需要在依次遍历中删除a到b之间的值的链表，需要保存a值之前的链
    //表以及保存链表之b之后的，然后将list2插入到这个过程中
    //整体过程来说，就是O(N)
    public ListNode mergeInBetween(ListNode list1, int a, int b, ListNode list2) {
        if (list1 == null || list2 == null) return null;
        ListNode node = list1;
        //遍历a之前链表 需要指向 list2
        for (int i = 0; i < a - 1; i++) {
            node = node.next;
        }
        ListNode cur = node;
        //将a-b之间链表删除  
        ListNode node2 = node.next;
        for (int i = 0; i < (b -a +1) ; i++) {
            node2 = node2.next;
        }

        //合并链表
        ListNode node3 = list2;
        cur.next = node3;
        while (node3.next != null) {
            node3 = node3.next;   
        }
        node3.next = node2;
        return list1;
    }
```

