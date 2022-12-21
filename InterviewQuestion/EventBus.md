
EventBus 怎么进行线程间通信
```
基于观察者模式的事件订阅/发布框架。
作用：
1、能够指定消息发往哪个线程。（不管你是从哪个线程发送过来的）

通过@Subscribe(threadMode = ThreadMode.MAIN) //这里是指定到主线程处理
fun onEventTest(event: TestEvent){

}
threadMode 包含以下的模式：
POSTING: 哪个线程发送就在哪个线程接收
MAIN: 切换到主线程进行接收
MAIN_ORDERED: 也是切换到主线程，但是和MAIN有区别（第二个订阅者
              需要第一个订阅者处理完，才能接收事件）
BACKGROUND: 确保在子线程中接收事件，如果在主线程发布事件，就新建一个子线程，否则不用新建。
ASYNC: 每次都在不同的线程中接收

使用：
普通事件
粘性事件

在onCreate中：
EventBus.getDefault().register(this);
在onDestroy中：
EventBus.getDefault.unRegister(this);

Fragment：
在onCreateView中注册
在onDestroyView中解注册

发射事件就很简单：
EventBus.getDefault().post(new StringEvent("hello"))

接收事件的地方：
@Subscribe（threadMode = = ThreadMode.MAIN）
fun onStringEvent(event: StringEvent){ //这个地方的方法名，可以随意
  //todo
}

发射粘性事件：
EventBus.getDefault().postSticky(new StringEvent("hello"))
接收事件的地方：
@Subscribe（sticky = true, threadMode = = ThreadMode.MAIN）
fun onStringEvent(event: StringEvent){
  //todo
  EventBus.getDefault().removeStickyEvent(stickyEvent)
}

```
[详细地址：https://juejin.im/post/5d81c6e8e51d4561ad65497c](https://juejin.im/post/5d81c6e8e51d4561ad65497c)

为什么发送粘性事件，没有提前注册也能接收到事件？
```
粘性事件，保存在map中
注意：
在使用完事件之后，要及时将事件清除，避免后面还会多次执行。
EventBus.getDefault().removeStickyEvent(stickyEvent);
```
[详细地址：https://juejin.cn/post/6844903969395851272](https://juejin.cn/post/6844903969395851272)
如何避免，多个地方接收问题？
```
一般是使用了相同的类。
解决：
1、每个事件都要定义一个不同的类名，这样就做到区分，避免重复。
2、另外加个参数，用来区分事件来源。
```