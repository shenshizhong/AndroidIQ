 1、Handler机制Looper死循环为什么不会导致应用卡死？
 ```
 对于线程即是一段可执行的代码，当可执行代码执行完成后，线程生命周期便该终止了，线程退出。
 而对于主线程肯定不能运行一段时间后就自动结束了，那么如何保证一直存活呢？？简单的做法就是
 可执行代码能一直执行下去，死循环便能保证不会被退出，例如：binder线程也是采用死循环方法，
 通过循环方式不同与Binder驱动进行读写操作，当然并非简单的死循环，无消息时会休眠，但是死
 循环又如何处理其他事物呢？？通过创建新的线程。真正卡死主线程操作的是在回调方法onCreate、
 onStart、onResume等操作时间过长，会导致掉帧甚至ANR，Looper.loop()本身不会导致应用卡死。
```
主线程的死循环一直在运行，是不是特别消耗CPU资源？
```
这里涉及Linux pipe/epoll(也就是管道和IO事件的通知机制)。
简单的说就是MessageQueue 没有消息的时候，就阻塞在loop的queue.next中的
nativePollOne() 方法里。这个时候主线程就会释放CPU资源进入休眠状态。
那么主线程怎么被唤醒的呢？
等到下一个消息或者有事务操作的时候，通过pipe管道写入数据来唤醒主线程工作。
这采用的epoll机制（也就是IO事件通知机制），是一种IO多路复用机制，可以同时控制
多个描述符，当某个描述符就绪（读或写准备就绪），则通知相应的程序进行读或者写操作，
本质同步I/0，所以读写是阻塞的。所以主线程大多数情况下是处于休眠的状态的，并不会
消耗大量的CPU资源。

```


 android 主线程是用来做什么的？
```
 主线程主要是用来运行四大组件，以及处理它们和用户的额交互。
 子线程主要用来执行耗时的操作，比如网络请求，i/O操作(也就是文件输入输出流的操作)。
```
[详细地址：https://blog.csdn.net/weixin_38196407/article/details/106426942](https://blog.csdn.net/weixin_38196407/article/details/106426942)


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

loop 执行的时候，是怎么回调Handler中的 handlerMessage()?
```
sendMessageDelayed()->sendMessageAtTime()->enqueueMessage() 这里是关键，
private boolean enqueueMessage(@NonNull MessageQueue queue,
@NonNUll Message msg, long uptimeMills){
    msg.target = this;//这里是关键，这里是把当前的Handler赋值给了msg.target
}

再看Looper.class 中的 loop 方法：
public static void loop(){
     for(;;){
        Message msg = queue.next; //可能阻塞
        ...
        try{
          msg.target.dispatchMessage(msg)  //这里我们拿到了Handler进行了调用
        }catch{
        }
     }
}

Handler 中的方法 dispatchMessage():
public void dispatchMessage(@NonNUll Message msg){
    if(msg.callback != null){
        handleCallBack(msg);
    }else{
      if(mCallback != null){
        if(mCallback.handlerMessage(msg){
          return;
        }
      }
      handlerMessage(msg)  //这里就回调了，一般情况下，我们在Activity中定义的 Handler 中的handlerMessage方法。
    }
}
总的来说：就是在enqueueMessage（）中进行赋值 -> 在loop中取出该对象，并进行回调。

```