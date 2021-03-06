1、App启动的优化？
```
App启动的方式有三种：
冷启动：App没有启动过或App进程被killed, 系统中不存在该App进程, 此时启动App即为冷启动。
热启动：热启动意味着你的App进程只是处于后台, 系统只是将其从后台带到前台, 展示给用户。
 
介于冷启动和热启动之间, 一般来说在以下两种情况下发生:
 
(1)用户back退出了App, 然后又启动. App进程可能还在运行, 但是activity需要重建。
(2)用户退出App后, 系统可能由于内存原因将App杀死, 进程和activity都需要重启, 
   但是可以在onCreate中将被动杀死锁保存的状态(saved instance state)恢复。
 
优化：
Application的onCreate（特别是第三方SDK初始化），首屏Activity的渲染都不要进行耗时操作，如果有，就可以放到子线程或者IntentService中
```
2、布局的优化？
```
尽量不要过于复杂的嵌套。可以使用<include>，<merge>，<ViewStub>
使用约束布局，简化布局
```

3、响应优化？
```
Android系统每隔16ms会发出VSYNC信号重绘我们的界面(Activity)。
页面卡顿的原因：
(1)过于复杂的布局.
(2)UI线程的复杂运算
(3)频繁的GC,导致频繁GC有两个原因:
   1、内存抖动, 即大量的对象被创建又在短时间内马上被释放.
   2、瞬间产生大量的对象会严重占用内存区域
```
4、电池使用优化
```
(使用工具：Batterystats & bugreport)
(1)优化网络请求
(2)定位中使用GPS, 请记得及时关闭
 
网络优化(网络连接对用户的影响:流量,电量,用户等待)可在Android studio下方logcat
旁边那个工具Network Monitor检测

API设计：App与Server之间的API设计要考虑网络请求的频次, 资源的状态等. 
        以便App可以以较少的请求来完成业务需求和界面的展示.
Gzip压缩：使用Gzip来压缩request和response, 减少传输数据量, 从而减少流量消耗.
图片的Size：可以在获取图片时告知服务器需要的图片的宽高, 以便服务器给出合适的图片, 避免浪费.
网络缓存：适当的缓存, 既可以让我们的应用看起来更快, 也能避免一些不必要的流量消耗.

```
5、图片的优化？
```
(1)对图片本身进行操作。尽量不要使用setImageBitmap、setImageResource、
   BitmapFactory.decodeResource来设置一张大图，因为这些方法在完成decode后，
   最终都是通过java层的createBitmap来完成的，需要消耗更多内存.
(2)图片进行缩放的比例，SDK中建议其值是2的指数值,值越大会导致图片不清晰。
(3)不用的图片记得调用图片的recycle()方法
```
6、怎么缩小APK包大小呢？
```
1、build.gradle 中启用压缩、混淆和优化功能
    minifyEnabled true        //压缩代码
    shrinkResources true      //去掉不用的资源文件
    resConfig  "en"           //指定语言，移除其他不用的备用语言
    proguardFiles             //使用proguard混淆代码，混淆处理的目的是通过
                                缩短应用的类、方法和字段的名称来减小应用的大小。
2、tinypng对图片进行进一步的压缩预处理。
   aaptOptions {
    cruncherEnabled = false   //关闭Android Studio的PNG合法性检查的，直接不让它检查
                              //如果你的图片已经优化过了， 再次使用 cruncher 
                                可能会导致图片变大。
   }
3、图片资源选择xxhdpi即可
4、能用代码绘制实现的功能，尽量不要使用大量的图片
5、使用WebP图片，可以替代 JPEG 和 PNG 格式图片并且通常可以减少 30% 的文件大小
```
[官方地址：https://developer.android.com/topic/performance/reduce-apk-size.html](https://developer.android.com/topic/performance/reduce-apk-size.html)

7、内存使用上的优化有哪些？
```
1、避免创建不必要的对象
   优先考虑使用StringBuffer或者StringBuilder来进行拼接，而不是加号连接符。
   尽量使用基本数据类型来代替封装数据类型
   尽可能地少创建临时对象，越少的对象意味着越少的GC操作。
2、内存泄漏
   可以使用IntentService，当后台任务执行结束后会自动停止，避免了Service的内存泄漏。
3、重写Activity的onTrimMemory()，可以在手机内存降低的时候及时通知我们，
   根据传入的级别来去决定如何释放应用程序的资源。
4、读取一个Bitmap图片的时候，可以进行图片压缩。
5、不要过多的使用枚举，枚举占用的内存空间要比静态常量大（枚举类会创建多个实例）
6、常量用static final 来修饰（只分配一次内存，相当于全局变量）
7、使用android 特有的数据结构，比如 SparseArray 和 Pair
8、尽量采用静态内部类，避免内部类潜在的内存泄漏

```
[官方建议：https://developer.android.com/topic/performance/memory#Overhead](https://developer.android.com/topic/performance/memory#Overhead)

[枚举类占用内存原因：https://blog.csdn.net/xiao_nian/article/details/80002101](https://blog.csdn.net/xiao_nian/article/details/80002101)

8、如何使用工具检查应用的性能？
```
使用as自带的工具 profiler
1、可以检查cpu使用情况
2、内存使用情况
3、网络请求速度
4、耗电量情况
```
[android-profiler 的使用：](https://developer.android.com/studio/profile/android-profiler?hl=zh-cn)