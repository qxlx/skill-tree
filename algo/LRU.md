# LRU





> #### [146. LRU缓存机制](https://leetcode-cn.com/problems/lru-cache/)
>
> 难度中等536
>
> 运用你所掌握的数据结构，设计和实现一个  [LRU (最近最少使用) 缓存机制](https://baike.baidu.com/item/LRU)。它应该支持以下操作： 获取数据 `get` 和 写入数据 `put` 。
>
> 获取数据 `get(key)` - 如果密钥 (key) 存在于缓存中，则获取密钥的值（总是正数），否则返回 -1。
> 写入数据 `put(key, value)` - 如果密钥已经存在，则变更其数据值；如果密钥不存在，则插入该组「密钥/数据值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。
>
>  
>
> **进阶:**
>
> 你是否可以在 **O(1)** 时间复杂度内完成这两种操作？
>
>  

![image-20200808185440393](e:\pic\image-20200808185440393.png)



```java
private class Node{
        int val;
        int key;
        Node pre;
        Node next;

        public Node(int key,int val) {
            this.key = key;
            this.val = val;
        }
    }

    //添加在头结点之后 和 尾节点之前
    public void add(Node node){
        node.pre = head;
        node.next = head.next;

        head.next.pre = node;
        head.next = node;
    }

    //删除节点  将当前节点的pre和next指针改变
    public void removeNode(Node node){
        Node pre = node.pre;
        Node next =  node.next;

        pre.next = next;
        next.pre = pre;

    }

    //删除头结点-先删除节点-然后在添加到头结点中
    public void removeToHead(Node node){
        removeNode(node);
        add(node);
    }

    //将尾部元素删除-如果当前cache满了之后
    public Node popTail(){
        Node res = tail.pre;
        removeNode(res);
        return  res;
    }

    public Node head;
    public Node tail;
    private Hashtable<Integer,Node> cache = new Hashtable<>();
    private int size;
    private int capacity;

    public LRUCache(int capacity) {
        head = new Node(-1,-1);
        tail = new Node(-2,-2);
        //指定head tail的关联关系
        head.next = tail;
        tail.pre = head;
        size = 0;
        this.capacity = capacity;
    }
    
    public int get(int key) {
        Node node = cache.get(k);
        if (node == null){
            return -1;
        }
        //添加到头部
        add(node);

        return node.val;
    }
    
    public void put(int key, int value) {
        Node node = cache.get(k);

        if(node == null){
            Node newNode = new Node(k,v);
            //添加到hashtable中
            cache.put(k,newNode);
            add(newNode);
            ++size;

            if(size > capacity){
                //从链表中删除
                Node tail = popTail();
                //从hashtable中删除
                cache.remove(tail, k);
                size--;
            }
        }else{
            //移除头部
            node.val = v;
            removeToHead(node);
        }
    }
```



```java
private Map<Integer,Integer> map;

    public LRUCache(int capacity) {
        map = new LinkedCappedHashMap<Integer,Integer>(capacity);
    }
    
    public int get(int key) {
        if(!map.containsKey(key)){
            return -1;
        }
        return map.get(key);
    }
    
    public void put(int key, int value) {
        map.put(key,value);
    }

    public static class LinkedCappedHashMap<K,V> extends LinkedHashMap<K,V>{
        int maxCapacity;

        public LinkedCappedHashMap(int maxCapacity){
            //true false的区别就是 是否改变顺序 
            super(16,0.75f,true);
            this.maxCapacity = maxCapacity;
        }

        protected boolean removeEldestEntry(Map.Entry eldest){
            return size() > maxCapacity;
        }
    }
```

