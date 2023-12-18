# LinkedBlockingQueue

- LinkedBlockingQueue是一个由**链表**实现的有界队列阻塞队列。
- 新元素插入到队列的尾部，队列获取操作则是从队列头部开始获得元素
- 大小默认值为Integer.MAX_VALUE，所以我们在使用LinkedBlockingQueue时建议手动传值，为其提供我们所需的大小，避免队列过大造成机器负载或者内存爆满等情况。

## 基本特征

LinkedBlockingQueue使用的是两个lock锁进行判断的，而array是使用一个lock锁，所以liked的并发度要高性能更好

## 构造函数

```java
    public LinkedBlockingQueue() {
        this(Integer.MAX_VALUE);
    }
    
 		public LinkedBlockingQueue(int capacity) {
        if (capacity <= 0) throw new IllegalArgumentException();
        this.capacity = capacity;
        last = head = new Node<E>(null);
    }
```

## 基本属性

```java
 		//存储数据的节点
    static class Node<E> {
        E item;

        Node<E> next; // 单链表

        Node(E x) { item = x; }
    }
    
    //容量
    private final int capacity;

    private final AtomicInteger count = new AtomicInteger();

    //头节点
    transient Node<E> head;

    //尾部节点
    private transient Node<E> last;

    // 获取并移除元素时使用的锁，如take, poll, etc
    private final ReentrantLock takeLock = new ReentrantLock();

    //notEmpty条件对象，当队列没有数据时用于挂起执行删除的线程
    private final Condition notEmpty = takeLock.newCondition();

    // 添加元素时使用的锁如 put, offer, etc
    private final ReentrantLock putLock = new ReentrantLock();

    // notFull条件对象，当队列数据已满时用于挂起执行添加的线程
    private final Condition notFull = putLock.newCondition();

```

可以发现LinkedBlockingQueue使用的是两个lock锁进行并发控制的，添加和删除可以同时进行。并且本身是使用node链表节点进行处理的。默认值大小是Integer.MAX_VALUE。

## 添加

因为LinkedBlockingQueue继承了抽象类AbstractQueue，所以add方法自己没有实现，使用的是父类的。

```java
    public boolean add(E e) {
        if (offer(e))
            return true;
        else
            throw new IllegalStateException("Queue full");
    }
```

```java
    public boolean offer(E e) {
        //空处理
        if (e == null) throw new NullPointerException();
        final AtomicInteger count = this.count;
        //长度等于容量 返回 false
        if (count.get() == capacity)
            return false;
        int c = -1;
        //构建节点
        Node<E> node = new Node<E>(e);
        final ReentrantLock putLock = this.putLock;
        //获取锁
        putLock.lock();
        try {
            if (count.get() < capacity) {
                enqueue(node); // 添加元素
                //CAS 添加元素个数
                c = count.getAndIncrement();
                if (c + 1 < capacity)
                    //如果容量没有满，唤醒获取lock阻塞的线程，继续添加元素
                    notFull.signal(); // ？？ 怎么唤醒的
            }
        } finally {
            putLock.unlock();
        }
        if (c == 0)
            //如果存在数据 唤醒消费锁
            signalNotEmpty();
        return c >= 0;
    }
```



## 获取

```java
    public void put(E e) throws InterruptedException {
        if (e == null) throw new NullPointerException();
        int c = -1;
        Node<E> node = new Node<E>(e);
        final ReentrantLock putLock = this.putLock;
        final AtomicInteger count = this.count;
        putLock.lockInterruptibly();
        try {
            //队列满，等待notFull条件满足
            while (count.get() == capacity) {
                notFull.await();
            }
            //入队
            enqueue(node);
            c = count.getAndIncrement();
            if (c + 1 < capacity)
                notFull.signal();
        } finally {
            putLock.unlock();
        }
        if (c == 0)
            signalNotEmpty();
    }
```

```java
    public E poll() {
        //获取当前元素的个数
        final AtomicInteger count = this.count;
        //为空的话 返回null
        if (count.get() == 0)
            return null;
        E x = null;
        int c = -1;
        final ReentrantLock takeLock = this.takeLock;
        takeLock.lock();
        try {
            if (count.get() > 0) {
                x = dequeue();
                c = count.getAndDecrement();
                //如果队列未空 继续唤醒等待条件对象notEmpty上的消费线程
                if (c > 1)
                    notEmpty.signal();
            }
        } finally {
            takeLock.unlock();
        }
        if (c == capacity)
            signalNotFull();
        return x;
    }


    public E take() throws InterruptedException {
        E x;
        int c = -1;
        final AtomicInteger count = this.count;
        final ReentrantLock takeLock = this.takeLock;
        takeLock.lockInterruptibly();
        try {
            while (count.get() == 0) {
                notEmpty.await();
            }
            x = dequeue();
            c = count.getAndDecrement();
            if (c > 1)
                notEmpty.signal();
        } finally {
            takeLock.unlock();
        }
        if (c == capacity)
            signalNotFull();
        return x;
    }
```

## 对比

- 队列大小不一样，array是有界队列，Linked是无界队列，后者可能出现OOM
- 数据结构不一样，array是数组，linked是使用链表
- 并发度不一样，array是一个lock，linked是两个lock