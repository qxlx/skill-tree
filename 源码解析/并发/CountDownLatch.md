## 使用案例

CountDownLatch的用法是通过构造参数输入需要等待的线程个数，countDown 进行操作，当state=0的时候，阻塞的await线程就可以继续执行任务。

```java

    public static void main(String[] args) {
        CountDownLatch countDownLatch = new CountDownLatch(2);

        Thread t1 = new Thread(() -> {
            countDownLatch.countDown();
            System.out.println(Thread.currentThread().getName()+ " 线程执行了");
        },"t1");

        Thread t2 = new Thread(() -> {
            countDownLatch.countDown();
            System.out.println(Thread.currentThread().getName()+ " 线程执行了");
        },"t2");

        t1.start();
        t2.start();

        Thread t3 = new Thread(() -> {
            try {
                countDownLatch.await();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName()+ " 线程执行了");
        },"t3");

        Thread t4 = new Thread(() -> {
            try {
                countDownLatch.await();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName()+ " 线程执行了");
        },"t4");

        t3.start();
        t4.start();
    }
```

## 构造方法

通过构造参数，可以发现其实内部通过syn进行初始化一个内部对象。实现了AQS。也就是通过构造方法将state设置为对应的数值2

```java
    public CountDownLatch(int count) {
        if (count < 0) throw new IllegalArgumentException("count < 0");
        // 构造一个内部sync
        this.sync = new Sync(count);
    }
    
     //内部类方法 实现类AQS
    private static final class Sync extends AbstractQueuedSynchronizer {}
 
 
        Sync(int count) {
            //继承是AQS类 将当前state 设置为count
            setState(count);
        }
```

## await

```java
    //当state = 0的时候，不阻塞
    public void await() throws InterruptedException {
        sync.acquireSharedInterruptibly(1);
    }
```

先判断当前线程是否中断，中断直接抛出异常

```java
    public final void acquireSharedInterruptibly(int arg)
            throws InterruptedException {
        //是否中断
        if (Thread.interrupted())
            throw new InterruptedException();
        // 线程等待
        if (tryAcquireShared(arg) < 0)
            doAcquireSharedInterruptibly(arg);
    }
```

getState() 此时等于2 所以返回-1 进入doAcquireSharedInterruptibly这个逻辑中

```java
    // 返回负数 获取锁失败
    // 返回0 获取锁成功  不唤醒后续节点
    // 返回正数 获取锁成功， 唤醒后续节点
    protected int tryAcquireShared(int arg) {
        throw new UnsupportedOperationException();
    }
    
        // state == 0 锁空闲，直接返回1
        protected int tryAcquireShared(int acquires) {
            return (getState() == 0) ? 1 : -1;
        }
```



doAcquireSharedInterruptibly的主要作用就是将当前线程封装成一个Node节点。加入到AQS队列中，注意是共享模式

```java
 private void doAcquireSharedInterruptibly(int arg)
        throws InterruptedException {
        // 封装成一个node 加入AQS队列中 共享模式
        final Node node = addWaiter(Node.SHARED);
        boolean failed = true;
        try {
            //自选锁
            for (;;) {
                final Node p = node.predecessor();
                if (p == head) {
                    // state 不等于0 返回-1
                    int r = tryAcquireShared(arg);
                    // 第一次不会进入
                    if (r >= 0) {
                        //  // 2. 这里将唤醒t3的后续节点t4,以此类推，t4被唤醒后，会在t4的await中唤醒t4的后续节点
                        setHeadAndPropagate(node, r);
                        // t3节点删除
                        p.next = null; // help GC
                        failed = false;
                        return;
                    }
                }
                // 修改前驱节点waitstate = -1 挂起当前线程
                if (shouldParkAfterFailedAcquire(p, node) &&
                    parkAndCheckInterrupt())
                    throw new InterruptedException();
            }
        } finally {
            if (failed)
                cancelAcquire(node);
        }
    }
```



## countDown 

## 使用场景



## 总结

