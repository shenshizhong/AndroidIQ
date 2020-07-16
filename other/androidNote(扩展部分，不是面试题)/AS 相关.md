
gradle 插件版本说明
```
编译出错的时候，检查gradle 对应的版本
```
[详细地址：https://developer.android.com/studio/releases/gradle-plugin.html#updating-gradle](https://developer.android.com/studio/releases/gradle-plugin.html#updating-gradle)

最新AS的使用
```
会有很多AS新的功能的介绍
```
[详细地址：https://developer.android.com/studio/releases](https://developer.android.com/studio/releases)

创建类的模版声明
```
VISIBILITY   ->   PUBLIC 或 PACKAGE_PRIVATE
ABSTRACT     ->   TRUE 或 FALSE
FINAL        ->   TRUE 或 FALSE

```
[详细地址：https://developer.android.com/studio/write/create-java-class?hl=zh-cn](https://developer.android.com/studio/write/create-java-class?hl=zh-cn)

1、搜索 WebViewClient 相关类的命令
```
搜索命令：
find . -name '*.jar' -exec zipgrep -i WebViewClient {} \;
```