
IntentFilter是什么？有哪些使用场景？
```
意图过滤，可以给四大组件配置自己关心的action等，
避免想打开A结果打开了B。
使用的场景：比如Receiver 需要通过IntentFilter来表明自己关心什么广播，只有匹配了才能触发自己。
还有就是app中的第一个启动页activity。
<intent-filter>
  <action android:name="android.intent.action.MAIN"> //决定程序最先启动的Activity
  <category android: name ="android.intent.category.LAUNCHER"> //决定应用程序 是否显示在 程序列表里
</intent-filter>
```
[详细地址:https://blog.csdn.net/jackrex/article/details/9189657](https://blog.csdn.net/jackrex/article/details/9189657)