1、LeakCanary的工作过程以及原理？
```
在一个Activity执行完onDestroy()之后，将它放入WeakReference中，然后将这个
WeakReference类型的Activity对象与ReferenceQueque关联。这时再从ReferenceQueque
中查看是否有没有该对象，如果没有，执行gc，再次查看，还是没有的话则判断发生内存泄露了。
最后用HAHA这个开源库去分析dump之后的heap内存。


如果弱引用referent持有的对象被GC回收，JVM就会把这个弱引用加入到与之关联的引用
队列referenceQueue中。即 KeyedWeakReference 持有的 Activity 对象如果被GC
回收，该对象就会加入到引用队列 referenceQueue 中

这里会判断 retainedKeys 集合中是否还含有 reference，若没有，证明已经被回收了，
若含有，可能已经发生内存泄露（或Gc还没有执行回收）

前面的分析中我们知道了 reference 被回收的时候，会被加进 referenceQueue 里面，
然后我们会调用removeWeaklyReachableReferences()遍历 referenceQueue 移除掉 
retainedKeys 里面的 refrence。

使用haha库解析 hprof文件


另外的版本：
手动触发GC，利用弱引用被回收会被放入引用队列的机制。将引用队列里的被回收对象出列，
并从Set集合中移除对应的key值。

如果Set集合中监控对象的key值没有被移除，说明监控对象没有被放入引用队列，
即没有被回收，发生了内存泄漏。

```
[详细地址 https://mp.weixin.qq.com/s?__biz=MzA5MzI3NjE2MA==&mid=2650243402&idx=1&sn=e7632788e8e147320b26a1b006cabf4c&chksm=88637025bf14f933dcf90fbd1d7f9090e3802dd37759cac014ac1ed73c455fb9b5d9953bb89f&scene=38#wechat_redirect](https://mp.weixin.qq.com/s?__biz=MzA5MzI3NjE2MA==&mid=2650243402&idx=1&sn=e7632788e8e147320b26a1b006cabf4c&chksm=88637025bf14f933dcf90fbd1d7f9090e3802dd37759cac014ac1ed73c455fb9b5d9953bb89f&scene=38#wechat_redirect)

[详细地址 https://www.jianshu.com/p/49239eac7a76](https://www.jianshu.com/p/49239eac7a76)

2、LeakCanary 回收机制算法
```
主要原理是是通过GCRoots对象作为根节点，从根节点开始往下遍历，遍历的路径称为引用链，
当一个对象到GC Roots没有任何引用链相连的时候，则说明此对象是不可用的。

```

```
什么是内存溢出？
就是没有足够的内存供其使用，就是内存溢出。比如申请了一个integer，
但是给了个long的数，就会内存溢出。
什么是内存泄漏？
就是内存被申请之后，无法释放已经申请的内存空间。内存泄漏堆积多的，
再多的内存迟早被占光。
```
源码分析
思考的问题：LeakCanary是怎么检查内存泄漏的。
```
什么是内存泄漏？
虚拟机判断了GC ROOT 不可达，又回收不了这个对象，那么就判断内存泄漏了
（也就是没有地方在用这个对象，而这对象还在占用这内存，导致了内存浪费，就叫内存泄漏）

怎么检查内存泄漏的？
在声明一个WeakReference对象时，同时传入 ReferenceQueue 作为构造参数，
当WeakReference 持有的对象被GC 回收时，JVM 会把这个WeakReference存入
与之关联的ReferenceQueue之中。依靠这个特性，我们就能检测内存泄漏了。
（也就是说，GC之后会把，持有的对象的WeakReference，存入到ReferenceQueue中）
比如，在onDestroy中，把 activity 存入到与 ReferenceQueue 关联
的 WeakReference 中，在一段时间主动GC几次，ReferenceQueue 如果一直为null
就说明发生了内存泄漏。

在LeakCanary 中呢，将 key 先保存到 watchedObjects，GC之后呢，
如果没有内存泄漏，那么watchedObjects 会被清空，如果有值，就说明发生了内存泄漏。
private fun removeWeaklyReachableReferences{
  var ref: KeyedWeakReference?
  do{
    ref = queue.poll() as KeyedWeakReference?
    if (ref != null){
        watchedObjects.remove(ref.key)
    }
  }while(ref != null)
}


LeakCanary怎么进行初始化的?
LeakCanary 是通过MainProcessAppWatcherInstaller（它是继承ContentProvider）
来自动完成的。
internal class MainProcessAppWatcherInstaller : ContentProvider{
  val application = context!!.applicationContext as Application
  AppWatcher.manualInstall(application)
}

LeakCanary 的原理是什么？
LeakCanary 通过监听Activity生命周期，在Activity onDestroy的时候，
创建一个弱引用，key跟当前Activity绑定，将key保存到set里面，
并且关联一个引用队列，然后在主线程空闲5秒后，开始检测是否内存泄漏，
具体检测步骤：
1：判断引用队列中是否有该Activity的引用，有则说明Activity被回收了，移除Set里面对应的key。
2：判断Set里面是否有当前要检测的Activity的key，如果没有，说明Activity对象已经被回收了，
   没有内存泄漏。如果有，只能说明Activity对象还没有被回收，可能此时已经没有被引用，
   不一定是内存泄漏。
3：手动触发GC，然后重复1和2操作，确定一下是不是真的内存泄漏。
[详细地址：https://www.jianshu.com/p/eb17a6bfdd9f](https://www.jianshu.com/p/eb17a6bfdd9f)
```
[详细地址：https://juejin.cn/post/7078237900583731213](https://juejin.cn/post/7078237900583731213)