
1、在java中调用kotlin 默认值
```
在kotlin 中就需要用@JvmOverloads 注解它，这个可以让编译器生成java要的重载函数。
比如：
String joinToString(Collection<T> collection);
String joinToString(Collection<T> collection, String separator)
...
[详细地址：https://www.jianshu.com/p/72d1959a7c56](https://www.jianshu.com/p/72d1959a7c56)
```
2、java 中调用扩展函数
```
假设函数声明在StringUtil.Kt文件中，java 是这么去调用的：
char c = StringUtilKt.lastChar("abcd")
```

java 中怎么调用顶层函数
```
kotlin 中顶层函数会被编译成静态函数，直接调用即可。

package strings
fun joinToString(...): String{...}

在java中调用如下：
JoinKt.joinToString(list,",","","");

怎么修改文件类名
@file：JvmName（"StringFunctions"）    //注解指定类名
package strings
fun joinToString(...): String{...}

StringFunctions.joinToString(list,",","","");   //在java 中的调用
```



--------------------------kotin 调用 java-------------------

kotlin 怎么定义常量？
```
const val UNIX_LINE_SEPARATOR = "\n"
等同
public static final string UNIX_LINE_SEPARATOR = "\n"
```

java 的getClass()
```
怎么用kotlin表示？用.javaClass

比如：
val set = hashSetOf(1,2,4)
>>>println(set.javaClass)
class java.util.HashSet

```
