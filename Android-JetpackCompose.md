# Jetpack Compose 布局入门

* [入门教程参考](https://developer.android.google.cn/jetpack/compose/tutorial?hl=zh-cn)
* [项目参考](https://github.com/android/nowinandroid?tab=readme-ov-file)
* [创建应用课程](https://developer.android.google.cn/jetpack/compose/tutorial?hl=zh-cn)

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

## 预览

*  可以多个模式预览
```
@Preview(name = "Light Mode") 
@Preview(
    uiMode = Configuration.UI_MODE_NIGHT_YES,
    showBackground = true,
    name = "Dark Mode"
)
@Composable
fun PreviewMessageCard() {

}

```

## 单元布局 
* Row  横向排列
* Column 纵向排列
* Box 重叠
* Space 间隔
* 

## 页面布局 
* 竖向列表布局 
* 横向列表布局
* 顶部导航布局
* 底部导航布局


## 拓展阅读
* [资源管理器](https://developer.android.google.cn/studio/write/resource-manager)
* [material3 应用于安卓的Api](https://developer.android.google.cn/reference/kotlin/androidx/compose/material3/package-summary)
* [material3 设计网](https://m3.material.io/foundations)
* [material1 设计网](https://m1.material.io/platforms/platform-adaptation.html#platform-adaptation-platform-recommendations)
* [material2](https://m2.material.io)



