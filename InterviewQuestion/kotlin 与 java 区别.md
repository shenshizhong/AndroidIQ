
kotlin 与 java 的区别
```
1、java 中的原始类是Object，而kotlin 是Any
Any 方法中只有equals(), hashCode(),toString()

2、kotlin 中的类型转换是as关键字。
3、
var intRange: IntRange = 0..1024 [0, 1024] 区间是全闭的
var intRange: IntRange = 0 until 1024 [0, 1024) 区间是左闭右开
4、 intArrayOf 和 arrayOf 申明数组。
5、除了成员方法，还有个叫函数
6、kotlin 中的默认访问权限是public
7、类默认是final 如果你的类要被继承，就要写open来进行修饰。
8、kotlin 没有Switch 而是取而代之用when， 它的判断值可以用表达式来进行判
9、class Student(var name: String, var age: Int) 默认就是主构造函数。
10、还有个data来进行修饰的，这个很好用。
    data class People(var name: String, var age: Int, var sex: Int)
    1、自动为成员生成 setter 和 getter
    2、会自动重写 equals()/ hashCode() 和 toString()
    3、data 类没有无参构造函数，就得使用NoArg() 和 allOpen()
    4、data 类默认是final的，无法被继承。
    怎么是使用NoArg 和 allOpen？
    noarg插件：为指定的类，在编译时提供一个无参的构造方法。
    allopen插件：使用指定的注解给类标注，无需写open关键字，让类可以被继承。
    创建一个注解类：
    annotation class NoArg{ }
    在app.build 中添加
    noArg{
      annotation("com.ssz.NoArg")
    }
    @NoArg
    data class People(var name:String,var age:Int,var sex:Int)
    var people = People::class.java.newInstance()
    经过以上几步，就能生成无参构造方法了。
    [详细地址：https://juejin.cn/post/6893033373834182669](https://juejin.cn/post/6893033373834182669)

11、在kotlin 的接口中可以写实现方法不报错，但在java中是不能写实现的。
12、在class的内部类中，需要用inner进行修饰，否则就默认是静态类（不持有当前类）
13、kotlin 的类名之外的函数，叫做包级函数，包括扩展函数，都是静态的。在编译的时候，
    kotlin 就把所有的包级函放在一个类中，java 可以通过Student.getMessage()来调用。
    class Student（var name: String, var age: Int）{
    }
    fun getMessage(){
      //todo
    }
14、kotlin 中的通配符不是？，而是*，？用来判断非空了
15、kotlin 支持jetpack compose （用代码写控件），而java 不支持
16、在kotlin 中没有static 而是用伴生类代替。
companion object{
  var a: Int = 3
  fun get(): Int{
    return a
  }
}
```
[详细地址：https://blog.csdn.net/luo_boke/article/details/107172965](https://blog.csdn.net/luo_boke/article/details/107172965)
