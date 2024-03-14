# Jetpack Compose 布局

* [教程参考](https://developer.android.google.cn/jetpack/compose/tutorial?hl=zh-cn)
* [项目参考](https://github.com/android/nowinandroid?tab=readme-ov-file)


## Jetpack Compose 描述

* Jetpack Compose 是用于构建原生 Android 界面的新工具包。它使用更少的代码、强大的工具和直观的 Kotlin API，可以帮助您简化并加快 Android 界面开发。

* Jetpack Compose 是围绕可组合函数构建的，创建可组合函数，只需将 @Composable 注解添加到函数名称中即可

* Jetpack Compose 使用 Kotlin 编译器插件将这些可组合函数转换为应用的界面元素。

## 引入
在要引入的函数上增加注解
```
@Composable
fun MessageCard(name: String) {
    Text(text = "Hello $name!")
}

```
## 排列
```
row
``` 


// 项目教程
