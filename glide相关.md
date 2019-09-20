
1、Kotlin和Java的区别？



- 
- 
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

 



