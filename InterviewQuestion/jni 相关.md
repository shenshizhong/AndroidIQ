1、JNI 与 NDK到底是什么？
```
JNI：
1、Java Native Interface，即 Java本地接口
2、使得Java 与 本地其他类型语言（如C、C++）交互
3、JNI 是属于 Java 的，与 Android 无直接关系
4、采用 JNI特性 增强 Java 与 本地代码交互的能力（Java 与 本地代码交互的能力非常弱）

NDK：
1、Native Development Kit，是 Android的一个工具开发包
2、NDK是属于 Android 的，与Java并无直接关系
3、快速开发C、 C++的动态库，并自动将so和应用一起打包成 APK


一句话：JNI 调用 NDK 开发的本地代码
```
[详细地址：https://blog.csdn.net/carson_ho/article/details/73250163](https://blog.csdn.net/carson_ho/article/details/73250163)