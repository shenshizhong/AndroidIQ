
android 系统平台的架构是什么样的呢？
```
android 的基础是Linux 内核
它是基于Linux 的开放源代码软件栈，为各类设备和机型而创建。
System Apps
(包含Email、Calendar、Camera 等等的系统应用)
Java API Framework
（Content Providers、View System、 Managers（Activity、Location、Package、Notification、Resource、Windows、Telephony））
Native c/c++ Libraries  Android Runtime
Hardware Abstraction Layer(HAL)
Linux Kernel

DEX 文件是什么？
DEX 是专门为android 设计的字节码格式

JVM 是java虚拟机，用来运行java 字节码。
ART 是什么呢？（Android Runtime）
ART 是新的android 运行时环境（老的叫Dalvik）
ART 是google 在android4.4 推出的，替换Dalvik 的新Android 运行时环境。

为什么有JVM还要有Dalvik 呢？
android 一般用的是java 语言写的，但Dalvik 并不支持Java的字节码。
所以需要对.class 文件进行翻译、压缩、重构、解释。
这个转化的过程是由dx进行处理的，转化后的文件就是.dex结尾的文件，也及时DEX文件。
这样一来DEX文件就能在Dalvik中运行了。

dx 是什么呢？
dx.jar，这个文件在sdk中，sdk根目录/build-tools/
作用是将.class 文件转化为.dex 文件

apk是什么呢？
包含一堆.dex 文件的包
jar是什么呢？
包含一堆.class 文件的包
[详细地址：https://cataloc.gitee.io/blog/2020/06/09/Dalvik%E8%99%9A%E6%8B%9F%E6%9C%BA/#Dalvik%E8%99%9A%E6%8B%9F%E6%9C%BA%E4%B8%8EJava%E8%99%9A%E6%8B%9F%E6%9C%BA%E7%9A%84%E5%8C%BA%E5%88%AB](https://cataloc.gitee.io/blog/2020/06/09/Dalvik%E8%99%9A%E6%8B%9F%E6%9C%BA/#Dalvik%E8%99%9A%E6%8B%9F%E6%9C%BA%E4%B8%8EJava%E8%99%9A%E6%8B%9F%E6%9C%BA%E7%9A%84%E5%8C%BA%E5%88%AB)


```
JVM 能够支持哪些编程语言呢？
```
JVM 不再只支持Java，像Groovy，Scala，Kotlin等

为什么能支持其他的语言呢？

本质上JVM 只能识别字节码文件（.class），但是呢，很多语言会通过对应的编译器，
转化成字节码文件，这样就能够在JVM中运行了。
java 源代码（.java）-> javac 编译器 -> 字节码文件（.class）
scala 源代码（.scala）-> scalac 编译器 -> 字节码文件（.class）
Groovy 源代码（.groovy）-> groovyc 编译器 -> 字节码文件（.class）
kotlin 源代码（.kt）-> kotlinc 编译器 -> 字节码文件（.class）
```
[详细地址：https://www.jianshu.com/p/f1014528197a](https://www.jianshu.com/p/f1014528197a)

