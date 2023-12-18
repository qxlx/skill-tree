## 公平锁和非公平锁

```java
    public ReentrantLock() {
        sync = new NonfairSync();
    }
```



```java
    public ReentrantLock(boolean fair) {
        sync = fair ? new FairSync() : new NonfairSync();
    }
```

非公平锁

```java
    //非公平锁
    static final class NonfairSync extends Sync {
        private static final long serialVersionUID = 7316153563782823691L;

        /**
         * Performs lock.  Try immediate barge, backing up to normal
         * acquire on failure.
         */
        final void lock() {
             //CAS
            if (compareAndSetState(0, 1))
                setExclusiveOwnerThread(Thread.currentThread());
            else
                acquire(1);
        }

        protected final boolean tryAcquire(int acquires) {
            return nonfairTryAcquire(acquires);
        }
    }
```

```java
if (!hasQueuedPredecessors() && compareAndSetState(0, acquires)) { //公平锁 需要判断当前队列是否有线程在等待
if (compareAndSetState(0, acquires)) {} //非公平锁 不需要
```

非公平锁，会先进行CAS获取锁，抢到了就直接返回。

## Condition

condition需要依赖于ReentrantLock，调用await 进入等待或者是singale唤醒，都需要获取锁才可以操作。

一个ReentrantLock可以创建多个condition条件。

```java
    public Condition newCondition() {
        return sync.newCondition();
    }

		final ConditionObject newCondition() {
        return new ConditionObject();
    }




public class ConditionObject implements Condition, java.io.Serializable {
        private static final long serialVersionUID = 1173984872572414699L;
  		 
        private transient Node firstWaiter;
        /** Last node of condition queue. */
        private transient Node lastWaiter;

        /**
         * Creates a new {@code ConditionObject} instance.
         */
        public ConditionObject() { }
```

