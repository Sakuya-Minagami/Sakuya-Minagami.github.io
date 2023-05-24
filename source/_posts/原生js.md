---
title: 原生js
---
打基础咯
[JavaScript高级程序设计](https://zhuanlan.zhihu.com/p/84326290)
# window对象
一个浏览器窗口就是一个window对象。
## 子对象
document 操作页面元素
location 操作url地址
navigator 获取浏览器版本信息
history 操作浏览器历史
screen 操作屏幕高宽
## api
open("url")
close()关闭当前窗口

弹出框confirm 确认返回true，取消返回false

单次定时器 setTimeout(function,time)
多次定时器 setInterval()

alert（）	弹出一个警告对话框
confirm（）	在确认对话框中显示指定的字符串
prompt（）	弹出一个提示对话框
open（）	打开新浏览器对话框，并且显示有URL或名字引用的文档，并设置创建对话框的属性
close（）	关闭被引用的对话框
focus（）	将被引用的的对话框放在所有打开对话框的前面
blur（）	将被引用的的对话框放在所有打开对话框的后面
scrollTo（x，y）	把对话框滚动到指定的坐标
scrollBy（x，y）	按照指定的位移量滚动对话框
setTimeout（timer）	在指定毫秒后，对传递的表达式求值
setInterval（interval）	指定周期性执行代码
moveTo（x，y）	将对话框移动到指定坐标处
moveBy（offsetx，offsety）	将对话框移动到指定的位移量处
resizeTo（x，y）	设置对话框大小
resizeBy（offsetx，offsety）	按照指定的位移量设置对话框的大小
print（）	相当于浏览器工具栏中“打印”按钮
status（）	状态条，位于对话框下部的信息条，用于任何时间内信息的提示（有些浏览器不支持）
defaultstatus（）	状态条，位于对话框下部的信息条，用于某个事件发生时的信息的提示（有些浏览器不支持）

### document
[Document对象](https://blog.csdn.net/u012060033/article/details/89713108)
### history
-   history.go(-1)等价于与history.back() —>返回之前的页面
-   history.go(1) 等价于history.forward() —>返回之后的页面
### location
reload刷新页面
replace('')替换页面，无法跳转回去
assign('')历史新增记录
href 直接赋值使用，修改地址
```
location.href='https://www.bilibili.com'
```
特别地，a标签的href属性赋值为name时为锚定url
如果写
```
<a href="javascript:void(0);" onclick=window.history.back()
```
可以取消a标签跳转功能，转为执行函数；也可直接把函数写在void内
### screen
height返回屏幕的总高度(以像素记)
width返回屏幕的总宽度(以像素记)
availHeight返回屏幕的总高度(不包括任务栏)
availWidth返回屏幕的宽度(不包括任务栏)
pixelDepth返回屏幕的颜色分辨率(每像素的位数)
