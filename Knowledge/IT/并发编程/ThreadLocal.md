
# 弱引用

Java中有四种应用方式

软引用：当发成gc，不一定会被清除。这个引用在内存不足时，才会被真正清除。

`强引用`， `软引用`，`弱引用`，`虚引用`

弱引用
`WeakReference`
当发生gc时，弱引用的对象就会被清除掉。


# ThreadLocal

在并发编程中，是线程安全的。
## 线程安全

如何做到安全：每个线程修改和读取`ThreadLocal`的值，在当前线程中是无法被其他线程干扰的。每个线程有一个`ThreadLocalMap`，这个`Map`
使用当前线程的`currentThread`做key，这样就能做到运行中不被其他线程干扰，相当于用key做了一把锁。

## 回收机制
 每当修改时，其实用了一个弱引用来获取ThreadLocal对象，当线程结束或者发成gc后，弱引用就会被回收


## 手动清理

```
ThreadLocal<Object> t = new ThreadLocal<>();
t.remove();
```