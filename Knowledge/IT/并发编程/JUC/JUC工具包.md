`java.util.concurrent`

# 常用工具速览

## **执行框架（Executor Framework）**  
这是 JUC 最核心的抽象。`Executor` 接口只定义了 `execute(Runnable)`，把“任务提交”与“任务如何运行”（新线程、线程池、调用线程）解耦。`ExecutorService` 增加了生命周期管理（`shutdown`）和 `Future` 返回能力。
`ThreadPoolExecutor` 和 `ScheduledThreadPoolExecutor` 是可配置的线程池实现，`Executors` 工厂类提供了常见配置的快捷方式。`ForkJoinPool` 是为分治任务（`ForkJoinTask`）设计的工作窃取线程池。

**异步结果与编排**  
`Future` 代表异步计算的结果，可以 `get()` 阻塞取值、`cancel()` 取消、`isDone()` 判断状态。
`CompletableFuture` 是 `Future` 的增强版，实现了 `CompletionStage`，支持链式回调、组合（`thenCombine`）、聚合（`allOf`/`anyOf`）和内建异常处理，是 Java 8 异步编排的核心工具。`CompletionService` 把“提交任务”和“按完成顺序获取结果”解耦，内部用一个阻塞队列存放已完成的任务。

**并发集合**  
`ConcurrentHashMap` 是高并发场景下替代 `Collections.synchronizedMap` 的首选，读操作几乎无锁，写操作分段/桶级别加锁。`ConcurrentLinkedQueue`/`ConcurrentLinkedDeque` 是非阻塞无界队列。`CopyOnWriteArrayList`/`CopyOnWriteArraySet` 适合读多写极少的场景，写时复制整个底层数组。`ConcurrentSkipListMap`/`Set` 提供有序的并发映射/集合。

**阻塞队列**  
`BlockingQueue` 接口定义了在队空时等待、队满时阻塞的 `put`/`take` 语义，是生产者-消费者模式的标准实现。常用实现：`ArrayBlockingQueue`（有界数组）、`LinkedBlockingQueue`（可选有界链表）、`SynchronousQueue`（不存储元素的交接队列）、`PriorityBlockingQueue`（优先级）、`DelayQueue`（延迟到期才能取出）。

**同步器（Synchronizers）**  
五个专用同步工具：`CountDownLatch`（一次性等待 N 个事件完成）、`CyclicBarrier`（可复用的线程栅栏，一组线程互相等待）、`Semaphore`（计数信号量，控制并发访问数量）、`Phaser`（更灵活的分阶段栅栏）、`Exchanger`（两个线程交换对象）。

**原子类与锁**  
`java.util.concurrent.atomic` 包提供 `AtomicInteger`、`AtomicLong`、`AtomicReference` 等，基于 CAS 实现无锁线程安全更新；`LongAdder` 在高竞争下比 `AtomicLong` 扩展性更好。`java.util.concurrent.locks` 包提供 `ReentrantLock`、`ReadWriteLock`、`Condition`，是 `synchronized`/`wait`/`notify` 的显式替代品，支持可中断锁、公平锁、超时获取等 `synchronized` 做不到的能力。

