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