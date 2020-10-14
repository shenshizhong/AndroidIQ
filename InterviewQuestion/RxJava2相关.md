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