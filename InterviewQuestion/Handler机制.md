 1、Handler机制Looper死循环为什么不会导致应用卡死？
 ```
 对于线程即是一段可执行的代码，当可执行代码执行完成后，线程生命周期便该终止了，线程退出。
 而对于主线程肯定不能运行一段时间后就自动结束了，那么如何保证一直存活呢？？简单的做法就是
 可执行代码能一直执行下去，死循环便能保证不会被退出，例如：binder线程也是采用死循环方法，
 通过循环方式不同与Binder驱动进行读写操作，当然并非简单的死循环，无消息时会休眠，但是死
 循环又如何处理其他事物呢？？通过创建新的线程。真正卡死主线程操作的是在回调方法onCreate、
 onStart、onResume等操作时间过长，会导致掉帧甚至ANR，Looper.loop()本身不会导致应用卡死。
 
 
 简单说就是在主线程的MessageQueue没有消息时，便阻塞在loop的queue.next()中
 的nativePollOnce()方法里，此时主线程会释放CPU资源进入休眠状态，直到下个消息到达
 或者有事务发生，通过往pipe管道写端写入数据来唤醒主线程工作。这里采用的epoll机制，
 是一种IO多路复用机制，可以同时监控多个描述符，当某个描述符就绪(读或写就绪)，
 则立刻通知相应程序进行读或写操作，本质同步I/O，即读写是阻塞的。 所以说，主线程大多数时候
 都是处于休眠状态，并不会消耗大量CPU资源。
```

2、Handler是怎么做到消息延时发送的
```
如果有延迟而且延迟时间没到的，计算下时间，保存为变量nextPollTimeoutMillis
然后在循环中会判断，如果这个Message有延迟，会调用nativePollOnce(ptr, nextPollTimeoutMillis)进行阻塞
```
[详细链接： https://blog.csdn.net/qingtiantianqing/article/details/72783952](https://blog.csdn.net/qingtiantianqing/article/details/72783952)

3、为什么默认创建出来的Handler是运行在主线程的？
```
使用空参数构建出来的Handler，默认使用UI线程中的Looper。
通过Looper.loop()取出来的消息在handMessage中进行处理时，是在UI线程中处理的。

```
为什么线程间可以进行通信？
```
主要是用了Looper

```
post 是在运行在哪个线程？
```
1、view 的 post方法，运行在主线程
2、handler 里面的post，运行在handler 依附的线程，可能是主线程，也可能是其他线程

注意点：如果是view.post, 由于不是在新的线程中使用，所有不要做耗时的操作（会增加主线程的工作量）。
```
[详细地址：https://blog.csdn.net/xiaoyu490697/article/details/7317396?spm=1001.2014.3001.5502](https://blog.csdn.net/xiaoyu490697/article/details/7317396?spm=1001.2014.3001.5502)
