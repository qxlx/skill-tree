## 阻塞队列

> 阻塞队列：顾名思义 首先它是一个队列，而一个阻塞队列在数据结构中所起的作用大致如下入所示。

- 当阻塞队列是空时，从队列中获取元素的操作将会被阻塞。
- 当阻塞队列时满的时，往队列里添加元素的操作将会被阻塞。

试图从空的阻塞队列中获取元素的线程将会被阻塞，直到其他的线程往空的队列插入新的元素。

同样，试图往已满的阻塞队列中添加新元素的线程同样也会被阻塞，直到其他的线程从队列中移除一个元素才可以插入队列中。

### 为什么使用阻塞队列 好处?

在多线程领域:所谓阻塞，在某些情况下会挂起线程(即阻塞)，一旦条件满足，被挂起的线程又会自动被唤醒。

**为什么需要BlockingQueue**

好处是我们不需要关心什么时候需要阻塞线程，什么时候需要唤醒线程，因为这一切BolckingQueue都给你一手包办了，在concurrent包发布以前，在多线程环境下，我们每个程序员都必须去自己控制这些细节，尤其还要兼顾效率和线程安全，而这会给我们的程序带来不小的复杂度。

### 阻塞队列接口和实现类

| **实现类**              |                           **特点**                           |
| ----------------------- | :----------------------------------------------------------: |
| **ArrayBlockingQueue**  |               **由数组结构组成的有界阻塞队列**               |
| **LinkedBlockingQueue** | **由链表结构组成的有界(但大小默认值有Integer.MAX_VALUE)阻塞队列** |
| PriorityBlockingQueue   |                 支持优先级排序的无界阻塞队列                 |
| DelayQueue              |             使用优先级队列实现的延迟无界阻塞队列             |
| **SynchronousQueue**    |         **不存储元素的阻塞队列，也既单个元素的队列**         |
| LinkedTransferQueue     |                 由链表结构组成的无界阻塞队列                 |
| LinkedBlockingDeque     |                 由链表结构组成的双向阻塞队列                 |

### BlockingQueue

```
//阻塞队列
public interface BlockingQueue<E> extends Queue<E> {
   
//   将指定的元素插入到此队列的尾部（如果立即可行且不会超过该队列的容量
//  在成功时返回 true，如果此队列已满，则抛IllegalStateException。
    boolean add(E e);

    //插入队列的尾部
    boolean offer(E e);

 		void put(E e) throws InterruptedException;

    //插入队列的尾部，可以设置等待时间，不成功抛出异常
    boolean offer(E e, long timeout, TimeUnit unit)
        throws InterruptedException;

    //移除对头部元素。如果没有元素会阻塞
    E take() throws InterruptedException;

    //移除对头元素
    E poll(long timeout, TimeUnit unit)
        throws InterruptedException;
}
```

**插入方法：**

　　add(E e) : 添加成功返回true，失败抛IllegalStateException异常
　　offer(E e) : 成功返回 true，如果此队列已满，则返回 false。
　　put(E e) :将元素插入此队列的尾部，如果该队列已满，则一直阻塞  // 推荐使用这个
**删除方法：**

　　remove(Object o) :移除指定元素,成功返回true，失败返回false
　　poll() : 获取并移除此队列的头元素，若队列为空，则返回 null
　　take()：获取并移除此队列头元素，若没有元素则一直阻塞。 //推荐使用这个

## ArrayBlockingQueue

内部是通过冲入锁reentrantLock和Condition条件队列实现的，即有公平访问和非公平访问两种方式。

一个测试demo

```java
public class BlockingArrayQueue {

    public static void main(String[] args) {
        BlockingQueue<String> blockingQueue = new ArrayBlockingQueue<>(1);
        new Thread(new Product(blockingQueue)).start();
        new Thread(new Consumer(blockingQueue)).start();
    }

}

class Product implements Runnable{

    private BlockingQueue<String> blockingQueue;

    public Product(BlockingQueue blockingQueue) {
        this.blockingQueue = blockingQueue;
    }

    @Override
    public void run() {
        while (true) {
            try {
                System.out.println("添加元素了");
                blockingQueue.put(String.valueOf(UUID.randomUUID()));
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class Consumer implements Runnable{

    private BlockingQueue<String> blockingQueue;

    public Consumer(BlockingQueue blockingQueue) {
        this.blockingQueue = blockingQueue;
    }

    @Override
    public void run() {
        while (true) {
            try {
                String take = blockingQueue.take();
                System.out.println("消费元素了  "+take);
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```



### 构造方法

```java
    //默认是非公平锁的方式
    public ArrayBlockingQueue(int capacity) {
        this(capacity, false);
    }

	  //可以通过参数进行设置是否使用公平锁
    public ArrayBlockingQueue(int capacity, boolean fair) {
        if (capacity <= 0)
            throw new IllegalArgumentException();
        this.items = new Object[capacity];
        //使用的是 ReentrantLock 去判断是否使用公平锁
        lock = new ReentrantLock(fair);
        //使用的condition 非空和非满的条件
        notEmpty = lock.newCondition();
        notFull =  lock.newCondition();
    }
```



### 属性

```java

    /** The queued items */
    // 存储数据的数组 使用的是一个object数组
    final Object[] items;

    /** items index for next take, poll, peek or remove */
    //获取数据的索引
    int takeIndex;

    /** items index for next put, offer, or add */
    //添加数据的索引
    int putIndex;

    /** Number of elements in the queue */
    //队列元素的个数
    int count;

    /** Main lock guarding all access */
    //控制并发访问的锁ReentrantLock
    final ReentrantLock lock;

    // notEmpty条件对象，用于通知take方法队列已有元素，可执行获取操作
    /** Condition for waiting takes */
    private final Condition notEmpty;

    // notFull条件对象，用于通知put方法队列未满，可执行添加操作
    /** Condition for waiting puts */
    private final Condition notFull;

    //迭代器
    transient Itrs itrs = null;

```

### 添加

```java
   //add方法 其实调用的是offer()
    //加入数据成功 返回true
    //加入失败就抛出异常
    public boolean add(E e) {
        return super.add(e);
    }

    public boolean offer(E e) {
        //判断是否为空
        checkNotNull(e);
        final ReentrantLock lock = this.lock;
        //加锁操作
        lock.lock();
        try {
            //如果当前队列满 返回false
            if (count == items.length)
                return false;
            else {
                // 添加元素到队列中
                enqueue(e);
                return true;
            }
        } finally {
            //释放锁
            lock.unlock();
        }
    }


    //阻塞方法
    public void put(E e) throws InterruptedException {
        checkNotNull(e);
        final ReentrantLock lock = this.lock;
        lock.lockInterruptibly(); //该方法可被中断
        try {
            //当队列元素和数组长度相等  当前线程挂起，添加到notFull条件队列中等待唤醒
            while (count == items.length)
                notFull.await();
            //不满则，直接添加元素
            enqueue(e);
        } finally {
            lock.unlock();
        }
    }
```



### 获取

```java
   public E poll() {
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            //队列元素为空 返回null
            return (count == 0) ? null : dequeue();
        } finally {
            lock.unlock();
        }
    }

    public E take() throws InterruptedException {
        final ReentrantLock lock = this.lock;
        lock.lockInterruptibly();
        try {
            while (count == 0)
                //如果没有元素 会进行阻塞
                notEmpty.await();
            return dequeue();
        } finally {
            lock.unlock();
        }
    }
    
    
    //直接返回队列头的元素 不进行修改
    public E peek() {
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            return itemAt(takeIndex); // null when queue is empty
        } finally {
            lock.unlock();
        }
    }
```

