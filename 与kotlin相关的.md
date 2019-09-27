
1、Kotlin和Java的区别？
```
- kotlin更安全  
如空引用由类型系统控制，你不会再遇到NullPointerException。

- kotlin更简洁  
如你可以用Lambda 表达式；在使用了apply plugin: 'kotlin-android-extensions'  
甚至可以不使用findviewbyid()，直接调用view；

- kotlin可以与Java100%交互  
Kotlin可与Java进行100％的互操作，允许在Kotlin应用程序中使用所有现有的 Android 库 ，
Kotlin的标准库更多的是对Java库的扩展。
```
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
3、在Kotlin中如何将String转换为Int？

```
fun main(args: Array) {
    val s: String = "Kotlin"
    var x = 10
    x = "8".toInt()
}

```
4、怎么理解kotlin中的函数？
```
Kotlin函数是类的函数，可以很容易地存储在变量和数据结构中，
并且可以作为参数传递并从其他高阶函数返回。 Kotlin中的示例函数声明和用法：
 fun double(x: Int): Int { return 2 * x } 
 val result = double(2) 
```
5、常用值范围表达式有哪些？
```
if (i in 1..10) { // 等价于: 1 <= i && i <= 10
    println(i)
}

for (i in 1 until 10) {   // i in [1, 10), 不包含 10
     println(i)
}

```
6、kotlin 的智能转换的特点是什么？
```
1、使用is来做类型检查，等同于Java中的instanceof
2、不需要强制类型转化，直接可以调用对应的方法

例子：
fun calcBill(bill: Bill): Double{
    if(bill is WaterBill){
        bill.water(); //不需要强制转换，直接调用
    }
   }

```
7、kotlin 中如何处理null异常？
```
新引入运算符“?:”，一旦实例为空就返回该运算符右边的表达式
```
8、kotlin 中数据类的作用？
```
数据类包含基本的数据类型, 它不包含任何功能函数。
```
7、kotlin 怎么做到null-safety？
```
Kotlin 的类型系统旨在从我们的代码中消除 NullPointerException
可能NPE的原因有以下：
1、显式调用 throw NullPointerException()；
2、使用了下文描述的 !! 操作符；
3、有些数据在初始化时不一致
4、企图访问平台类型的 null 引用的成员；（比如在java中）
5、Java 代码可能会向 Kotlin 的 MutableList<String> 中加入 null，
    这意味着应该使用 MutableList<String?> 来处理它；
    
6、由外部 Java 代码引发的其他问题

居于上面可能出现的NPE，koltin可以通过以下方式避免掉：
1、条件中检测 null
2、安全调用操作符，写作 ?.
3、通过 Elvis 操作符表达，写作 ?:
4、非空断言运算符（!!）
5、使用安全的类型转换 as？
```
[详细地址：https://www.kotlincn.net/docs/reference/null-safety.html](https://www.kotlincn.net/docs/reference/null-safety.html)

8、lateinit 和 by lazy 的区别？
```
lateinit 只用于变量 var，而 lazy 只用于常量 val.
lazy 应用于单例模式(if-null-then-init-else-return)，
而且当且仅当变量被第一次调用的时候，委托方法才会执行

```
9、怎么自定义一个高级函数？
```
高阶函数的特点：
1、参数是函数（参数是lambda，也就是函数）

   fun shop(girl: String，play:()-> Unit){
   
       println("女朋友：$girl")
       play()
       
   }
   
2、或者返回值是函数

   fun decor(func: ()-> Unit): Unit{
   
       fun funcPlus(){
           println("hello")
           func()
       }
       
       return funcPlus()
   }
怎么进行传参数的：
decor{ println("hello!")}       //传递的是一个lambda

decor{::hello}                  //传递一个普通函数
fun hello(){println("hello!")}

总的来说：
1、在kotlin中，函数是一种类型，我们可以把具体的函数当成对象。
2、lambda表达式就是一个匿名函数
3、高阶函数就是在函数的参数中，声明传入参数的类型和返回的类型

```
