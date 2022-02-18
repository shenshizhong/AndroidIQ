1、RxJva2如何处理内存泄漏？
```
1、一般的做法是订阅成功后，拿到Disposable对象，在Activity/Fragment销毁时,
调用Disposable对象的dispose()方法，将异步任务中断
Disposable disposable = Observable              
    .interval(0, 1, TimeUnit.SECONDS)  //开启一个定时器
    .subscribe(aLong -> {                       

    });  
//Activity/Fragment销毁时，中断RxJava管道     
if (disposable != null && !disposable.isDisposed()) {
    disposable.dispose();                            
}  

2、使用 RxLife
底层也是调用Disposable.dispose()
as(LifecycleOwnerowner owner) 方法，接收的是一个LifecycleOwner接口对象
Activity/Fragment就实现了这个接口

```
RxJava2 会内存泄漏而OkHttp一般不会。
```
RxJava2 是因为页面销毁，但RxJava2 还在进行耗时操作，没有取消请求，导致了内存泄漏。
OkHttp 直接在onStop 调用cancel  就可以了，就不会导致内存泄漏。
RxJava2 每次请求都得到 Observable 对象，要在onStop中unsubscribe取消，不利封装，代码量会很多，所以需要第三方来解决。

第三方使用RxLifecycle，但更建议用AutoDispose。（https://www.jianshu.com/p/4b25ac968afe）

```
[详细地址：https://www.songbingjia.com/tech/show-98306.html](https://www.songbingjia.com/tech/show-98306.html)