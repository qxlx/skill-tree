

## AQS结构

```java
		//头节点 当前持有锁的线程
    private transient volatile Node head;

    /**
     * Tail of the wait queue, lazily initialized.  Modified only via
     * method enq to add new wait node.
     */
    //每个进来的线程都插入到最后
    private transient volatile Node tail;

    /**
     * The synchronization state.
     */
    //代表当前锁的状态 0：表示没有占用 大于0代表有线程持有当前锁
    private volatile int state;

    //CAS设置值
    protected final boolean compareAndSetState(int expect, int update) {
        // See below for intrinsics setup to support this
        return unsafe.compareAndSwapInt(this, stateOffset, expect, update);
    }
```

AQS内部包装成一个Node节点 通过state标识线程的状态

```java
    //等待线程被包装成一个Node节点
    static final class Node {
        /** Marker to indicate a node is waiting in shared mode */
        //共享模式下
        static final Node SHARED = new Node();
        /** Marker to indicate a node is waiting in exclusive mode */
        //独享模式下
        static final Node EXCLUSIVE = null;

        /** waitStatus value to indicate thread has cancelled */
        //线程取消争抢这个锁
        static final int CANCELLED =  1;
        /** waitStatus value to indicate successor's thread needs unparking */
        //当前node的后继节点需要被唤醒
        static final int SIGNAL    = -1;
        /** waitStatus value to indicate thread is waiting on condition */
        static final int CONDITION = -2;

        static final int PROPAGATE = -3;

        //强半天获取不到锁，就取消等待
        volatile int waitStatus;

        //前驱节点的引用
        volatile Node prev;

        //后继节点的引用
        volatile Node next;

        //本线程
        volatile Thread thread;

        // 这个是在condition中用来构建单向链表
        Node nextWaiter;
    }
```



```java
		// 使用static，这样每个线程拿到的是同一把锁
    private static ReentrantLock reentrantLock = new ReentrantLock(true);

    public void createOrder() {
        // 比如我们同一时间，只允许一个线程创建订单
        reentrantLock.lock();
        // 通常，lock 之后紧跟着 try 语句
        try {
            // 这块代码同一时间只能有一个线程进来(获取到锁的线程)，
            // 其他的线程在lock()方法上阻塞，等待获取到锁，再进来
            // 执行代码...
        } finally {
            // 释放锁
            // 释放锁必须要在finally里，确保锁一定会被释放，如果写在try里面，发生异常，则有可能不会执行，就会发生死锁
            reentrantLock.unlock();
        }
    }
```



## Lock接口

```java
public interface Lock {
		
		//加锁
    void lock();
    //尝试获取锁
    boolean tryLock(long time, TimeUnit unit) throws InterruptedException;
    //释放锁
    void unlock();
    
    Condition newCondition();
}
```



```java
		public class ReentrantLock implements Lock, java.io.Serializable 
  
    public ReentrantLock() {
        sync = new NonfairSync();
    }
  
    public ReentrantLock(boolean fair) {
        sync = fair ? new FairSync() : new NonfairSync();
    }
```

通过构造参数进行区分使用公平锁还是非公平锁。默认是非公平锁。

```java
		//继承AQS 正在的获取和释放锁是由Sync的实现类来控制的。
    abstract static class Sync extends AbstractQueuedSynchronizer {
        private static final long serialVersionUID = -5179523762034025860L;

        abstract void lock();

      
        final boolean nonfairTryAcquire(int acquires) {
          //xxxxxx
        }

        protected final boolean tryRelease(int releases) {
          //xxxx
        }
    }
```



## Lock 加锁

```java
		//sync进行管理锁
    //公平锁
    static final class FairSync extends Sync {
        private static final long serialVersionUID = -3000897897090466540L;

        //争抢锁🔒
        final void lock() {
            acquire(1);
        }
    }
```

父类实现的acquire

```java
    //lock.lock()
    public final void acquire(int arg) {
        //tryAcquire(arg) true 获取锁成功直接结束
        //如果没有获取到锁，acquireQueued 会将线程压入队列中
        //!tryAcquire(arg)  没有获取到锁，将当前线程挂起
        //addWaiter
        if (!tryAcquire(arg) &&
            acquireQueued(addWaiter(Node.EXCLUSIVE), arg))
            selfInterrupt();
    }

    //因为FairSync实现了
    protected boolean tryAcquire(int arg) {
        throw new UnsupportedOperationException();
    }
```

```java
		protected final boolean tryAcquire(int acquires) {
            //获取当前线程
            final Thread current = Thread.currentThread();
            int c = getState();
            // c == 0 当前没有线程获取锁
            if (c == 0) {
                //当前是公平锁，先来后到
                //查看队列中是否有等待的线程
                //hasQueuedPredecessors 没有的话才可以获取线程
                //compareAndSetState CAS设置 有可能同时多个线程竞争锁
                if (!hasQueuedPredecessors() &&
                    compareAndSetState(0, acquires)) {
                    //表示获取到锁了，标记以下
                    setExclusiveOwnerThread(current);
                    //执行到这里 表示获取锁成功
                    return true;
                }
            }
            //当前有线程持有锁，先判断下 获取线程的锁是不是自己 也就是重入了
            //state += 1;
            else if (current == getExclusiveOwnerThread()) {
                int nextc = c + acquires;
                //check
                if (nextc < 0)
                    throw new Error("Maximum lock count exceeded");
                setState(nextc);
                return true;
            }
            //到这里表示没有获取到锁
            //1.尝试获取失败
            //2.也不是自己的可冲入锁
            return false;
        }
```



```java
    //队列中有等待的线程 返回true
    public final boolean hasQueuedPredecessors() {
        // The correctness of this depends on head being initialized
        // before tail and on head.next being accurate if the current
        // thread is first in queue.
        Node t = tail; // Read fields in reverse initialization order
        Node h = head;
        Node s;
        return h != t &&
            ((s = h.next) == null || s.thread != Thread.currentThread());
    }

    //CAS设置值
    protected final boolean compareAndSetState(int expect, int update) {
        // See below for intrinsics setup to support this
        return unsafe.compareAndSwapInt(this, stateOffset, expect, update);
    }

    protected final void setExclusiveOwnerThread(Thread thread) {
        //将当前线程设置为获取到线程
        //当前拥有锁的线程
        exclusiveOwnerThread = thread;
    }
```



```java
    //此方法的作用是将线程包装成node， 同时进入队列中
    //EXCLUSIVE是独占模式
    private Node addWaiter(Node mode) {
        Node node = new Node(Thread.currentThread(), mode);
        // Try the fast path of enq; backup to full enq on failure
        Node pred = tail;
        if (pred != null) {
            //自己的前驱节点为队尾节点
            node.prev = pred;
            //CAS更新
            if (compareAndSetTail(pred, node)) {
                pred.next = node;
                return node;
            }
        }
        //有竞争锁，加入队列失败
        enq(node);
        return node;
    }    

		//因为是公平锁，所以会按照链表的形式将当前任务添加到队列中
    final boolean acquireQueued(final Node node, int arg) {
        boolean failed = true;
        try {
            boolean interrupted = false;
            for (;;) {
                //获取node的prev节点
                final Node p = node.predecessor();
                // p == head 说明当前节点虽然进到了阻塞队列，但是是阻塞队列的第一个，因为它的前驱是head
                if (p == head && tryAcquire(arg)) {
                    // //到这里说明刚加入到等待队列里面的node只有一个，并且此时获取锁成功，设置head为node
                    setHead(node);
                    p.next = null; // help GC
                    failed = false;
                    return interrupted;
                }
                // // 到这里，说明上面的if分支没有成功，要么当前node本来就不是队头，
                // // 要么就是tryAcquire(arg)没有抢赢别人，继续往下看
                if (shouldParkAfterFailedAcquire(p, node) &&
                    parkAndCheckInterrupt())
                    interrupted = true;
            }
        } finally {
            if (failed)
                cancelAcquire(node);
        }
    }
```

```java
//当前线程没有抢到锁 是否需要挂起当前线程
    //第一个参数是前驱节点 第二个节点是当前线程的节点
    private static boolean shouldParkAfterFailedAcquire(Node pred, Node node) {
        int ws = pred.waitStatus;
        //前驱节点正常 -1 当前线程需要挂起
        if (ws == Node.SIGNAL)
            /*
             * This node has already set status asking a release
             * to signal it, so it can safely park.
             */
            return true;

//        136     // 前驱节点 waitStatus大于0 ，之前说过，大于0 说明前驱节点取消了排队。这里需要知道这点：
//        137     // 进入阻塞队列排队的线程会被挂起，而唤醒的操作是由前驱节点完成的。
//        138     // 所以下面这块代码说的是将当前节点的prev指向waitStatus<=0的节点，
//        139     // 简单说，如果前驱节点取消了排队，
//        140     // 找前驱节点的前驱节，往前循环总能找到一个waitStatus<=0的节点
        if (ws > 0) {
            /*
             * Predecessor was cancelled. Skip over predecessors and
             * indicate retry.
             */
            do {
                node.prev = pred = pred.prev;
            } while (pred.waitStatus > 0);
            pred.next = node;
        } else {
            /*
             * waitStatus must be 0 or PROPAGATE.  Indicate that we
             * need a signal, but don't park yet.  Caller will need to
             * retry to make sure it cannot acquire before parking.
             */
//            // 仔细想想，如果进入到这个分支意味着什么
//            157         // 前驱节点的waitStatus不等于-1和1，那也就是只可能是0，-2，-3
//            158         // 在我们前面的源码中，都没有看到有设置waitStatus的，所以每个新的node入队时，waitStatu都是0
//            159         // 用CAS将前驱节点的waitStatus设置为Node.SIGNAL(也就是-1)
            compareAndSetWaitStatus(pred, ws, Node.SIGNAL);
        }
        return false;
    }


		//
    private final boolean parkAndCheckInterrupt() {
        LockSupport.park(this);
        return Thread.interrupted();
    }
```

## Lock 解锁

线程如果没有获取到锁，那么会挂起，LockSupport.park(this); 等待唤醒。

```java
    public void unlock() {
        sync.release(1);
    }
```

```java
    public final boolean release(int arg) {
        if (tryRelease(arg)) {
            Node h = head;
            if (h != null && h.waitStatus != 0)
                unparkSuccessor(h);
            return true;
        }
        return false;
    }
```



```java
    protected boolean tryRelease(int arg) {
        throw new UnsupportedOperationException();
    }
```

```java
        protected final boolean tryRelease(int releases) {
            int c = getState() - releases;
            if (Thread.currentThread() != getExclusiveOwnerThread())
                throw new IllegalMonitorStateException();
            //是否完全释放锁
            boolean free = false;
            // c == 0 没有嵌套锁住了 可以释放
            if (c == 0) {
                free = true;
                setExclusiveOwnerThread(null);
            }
            setState(c);
            return free;
        }


    protected final void setExclusiveOwnerThread(Thread thread) {
        //将当前线程设置为获取到线程
        //当前拥有锁的线程
        exclusiveOwnerThread = thread;
    }
```



```java
    private void unparkSuccessor(Node node) {
        /*
         * If status is negative (i.e., possibly needing signal) try
         * to clear in anticipation of signalling.  It is OK if this
         * fails or if status is changed by waiting thread.
         */
        int ws = node.waitStatus;
        if (ws < 0)
            compareAndSetWaitStatus(node, ws, 0);

        /*
         * Thread to unpark is held in successor, which is normally
         * just the next node.  But if cancelled or apparently null,
         * traverse backwards from tail to find the actual
         * non-cancelled successor.
         */
        // 唤醒后继节点，但是有可能后继节点取消了等待
        // 从队尾往前找，找到waitStatus <= 0 的所有节点排在最前面的一个
        Node s = node.next;
        if (s == null || s.waitStatus > 0) {
            s = null;
            //
            for (Node t = tail; t != null && t != node; t = t.prev)
                if (t.waitStatus <= 0)
                    s = t;
        }
        if (s != null)
            //唤醒线程
            LockSupport.unpark(s.thread);
    }
```

这里存在并发问题：从前往后寻找不一定能找到刚刚加入队列的后继节点。

如果此时正有一个线程加入等待队列的尾部，执行到上面第7行，第7行还未执行，解锁操作如果从前面开始找 头节点后面的第一个节点状态为-1的节点，此时是找不到这个新加入的节点的，因为尾节点的next 还未指向新加入的node，但是从后面开始遍历的话，那就不存在这种情况。



## 小结

**锁状态**，通过state标记， 0 没有线程占用锁，state >= 代表有线程获取到锁，> 1说明是可冲入锁。 所以lock和unlock必须是配对的。

**线程的阻塞和解决阻塞**：lockSupport.park(thread)挂起线程，unpack()唤醒线程。

**阻塞队列：** 争抢的线程很多，所以需要将等待的线程通过一个queue进行管理这些线程。AQS是一个FIFO的队列，