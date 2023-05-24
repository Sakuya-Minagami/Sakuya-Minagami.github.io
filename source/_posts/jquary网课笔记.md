---
title: jquary网课笔记
---
顺便，
[阮一峰jquary笔记](https://www.ruanyifeng.com/blog/2011/07/jquery_fundamentals.html)
# dom对象与jquary包装集的区别及二者的转换
# 选择器
## 基础选择器
id，元素名，类，所有，组合选择
## 层次选择器
$(“M N”) 等价于 $(M).find(N)；表示选择后代。
$(“M>N”) 等价于 $(M).children(N)；表示选择子代。
$(“M~N”) 等价于 $(M).nextAll(N)；表示在兄弟之中选择后面所有兄弟。
$(“M+N”) 等价于 $(M).next(N)；表示在兄弟之中只选择后面那个兄弟
## 表单选择器
表单元素：input，button，checkbox，file，hidden，image，password，radio，reset，submit，text
```
$("input")//所有input
$(":input")//所有表单包括input
```
## 过滤选择器
first，last，even，odd，eq(index)，gt(index)，lt(index)
## 属性选择器
```
$('input');  //所有input元素
$('input[id]'); //input元素中，设置了id属性的所有元素。
$('input[id=btn1]'); //input元素中，id=btn1的元素。
$('input[id*=btn]'); //input元素中，id包含btn的所有元素。
$('input[id^=btn]'); //input元素中，id以btn开头的所有元素。
$('input[id$=btn]'); //input元素中，id以btn结尾的所有元素。
$('input[value][type=text]');   //同时满足多个属性条件，用[] []。input元素中，设置了value属性值，并且类型是text的所有元素。

```
# 元素属性与样式
  ## attr与prop方法的区别
  [attr & prop](https://blog.csdn.net/qq_45932447/article/details/109532349)
property 是 DOM 中的属性，是 JavaScript 里的对象；  
attribute 是 HTML 标签上的，它的值只能够是字符串；
对固有属性，二者均可
1.自定义属性，只能attr
2.返回值是布尔值（checked，selected，disabled）
```
$("ck1").attr("checked");//checked
$("ck1").prop("checked");//true
```
设置过属性值，prop返回true，attr返回属性值
未设置过，prop返回false，attr返回undefined

改值：(element,value)
注，样式叠加时取决于style中定义的先后顺序
行内样式优先级高
# 操作元素内容 添加与删除元素
## 赋值
常用非表单元素：div，span，a，h1~h6
非表单元素有html（）方法和text（）方法
表单元素均为val（）方法
## 增删元素
内部增加：
append/appendTo
prepend/prependTo
同级增加：
before,after
# 删除与遍历
## 删除
remove（）删整个元素
empty（）只删文本
## 遍历
each（function（index，element））
# jquary事件
## 事件绑定
```
$("btn2").bind("click mouseout",function(){

})//两事件绑同函数

//链式编程
$("btn2").bind("click mouseout",function(){}
.bind()
.bind()

$("btn2").bind({
	click:function(){
	},
	mouseout:function(){
	}
})//类似json
```