
1、在java中调用kotlin 默认值
```
在kotlin 中就需要用@JvmOverloads 注解它，这个可以让编译器生成java要的重载函数。
比如：
String joinToString(Collection<T> collection);
String joinToString(Collection<T> collection, String separator)
...
```
2、java 中调用扩展函数
```
假设函数声明在StringUtil.Kt文件中，java 是这么去调用的：
char c = StringUtilKt.lastChar("abcd")
```
java 的getClass()
```
怎么用kotlin表示？用.javaClass

比如：
val set = hashSetOf(1,2,4)
>>>println(set.javaClass)
class java.util.HashSet

```
