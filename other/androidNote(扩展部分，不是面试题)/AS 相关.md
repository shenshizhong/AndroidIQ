
AS 相关


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

Live TempLates（代码模板）
```
搜索 Live TempLates
Abbreviation  输入： SL
Description  输入： 打印日志
Template text： 输入： Log.e("ssz","=============$tag$" + "$content$")
记得 define 选择一下你要应用的区域 就行了
```
[详细地址：https://blog.csdn.net/u010126792/article/details/94581801](https://blog.csdn.net/u010126792/article/details/94581801)

一键格式化所有代码
```
两种方法：
一、
选中要格式化的包，点击右键，然后reformat code

二、
选中要格式化的包，点击code，然后reformat code


```
[详细地址：https://blog.csdn.net/xiyangyang8110/article/details/77697600](https://blog.csdn.net/xiyangyang8110/article/details/77697600)

Clean Project与 Rebuild Project 区别
```
Rebuild 会先执行 Clean 再执行其他，Rebuild 更彻底。

```
[详细地址：https://www.jianshu.com/p/7a18eea2415f](https://www.jianshu.com/p/7a18eea2415f)

AS 直接运行java文件报错
```
Could not create task ':app:Test5.main()'.
   > SourceSet with name 'main' not found.

解决方法：

项目—> .idea -> gradle.xml， 在这个文件中，加入一行代码就行：

 <GradleProjectSettings>
    <option name="delegatedBuild" value="false"/>    //增加这行代码就可以了
 </GradleProjectSettings>

```