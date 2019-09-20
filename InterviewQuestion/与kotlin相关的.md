
1、Kotlin和Java的区别？

- kotlin更安全  
如空引用由类型系统控制，你不会再遇到NullPointerException。

- kotlin更简洁
如你可以用Lambda 表达式；在使用了apply plugin: 'kotlin-android-extensions'  甚至可以不使用findviewbyid()，直接调用view；

- kotlin可以与Java100%交互
Kotlin可与Java进行100％的互操作，允许在Kotlin应用程序中使用所有现有的 Android 库 ，Kotlin的标准库更多的是对Java库的扩展。

2、Kotlin 中是没有 static 这个关键字，该如何处理呢？

- 这里需要用到 Kotlin 的伴生对象来处理。
- 2个例子如下：
```
class AppManager private constructor(){
 
    private val activityStack: Stack<Activity> = Stack()
 
    companion object {
        val instance:AppManager by lazy { AppManager() }
    }
 
    /*Activity入栈*/
    fun addActivity(activity: Activity){
        activityStack.add(activity)
    }
}
 
 
//调用
AppManager.instance.addActivity(this)
```

使用 const 来声明
```

class BaseConstant {
    companion object {
        //服务地址
        const val IMAGE_SERVER_ADDRESS = "http://google.com" 
 
        //SP表名
        const val TABLE_PREFS = "Kotlin_mall"
 
    }
}

```

 



