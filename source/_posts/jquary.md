---
title: jquary
---
jquary是一个JavaScript库
[jquary教程](https://www.runoob.com/jquery/jquery-intro.html)
[jquary官方文档](https://jquery.com/download/)
[jquary API](https://jquery.shaozhi.vip/)
# 引用/预备
cdn
```
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js">
```
特别地，还可以把自己的jquary函数封装成文件引用
```
<script src="my_jquery_functions.js"></script>
```

## 文档就绪
所有函数写在document ready函数中,等dom创建完才能执行函数
```
$(document).ready(function(){
// 开始写 jQuery 代码... 
});

另一种简洁写法
$(function(){ 
// 开始写 jQuery 代码... 
});
```

# 语法
## jquary选择器
**$(_selector_)._action_()**
selector选择符候选：this(注意只有this不用双引号包起来), p, p.test,#test , p:first, [href],a[traget='_blank']

注意，函数可以嵌套
```
$(document).ready(function(){ 
	$("button").click(function(){ 
		$("p").hide(); 
	}); 
});
```
## 效果与动画
### 隐藏显示 hide与show
hide方法：$(_selector_).hide(_speed,callback_);
speed为隐藏时间，callback为完成后执行的函数名
show方法同理
toggle方法可交替hide和show方法
### 淡入淡出 fade 滑动slide
fadeIn，fadeOut, 
fade Toggle, 
fade To(speed,opacity,callback)

slideDown，slideUp，slide Toggle
### animate动画
$(_selector_).animate({_params_}_,speed,callback_);
必需的 params 参数定义形成动画的 CSS 属性。
可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。

注意，所有属性名都要用驼峰命名法
特别地，可以对属性使用相对值，如+=50px

动画队列功能：连续animate函数会连续执行
```
$("button").click(function(){ var div=$("div"); div.animate({height:'300px',opacity:'0.4'},"slow"); div.animate({width:'300px',opacity:'0.8'},"slow"); div.animate({height:'100px',opacity:'0.4'},"slow"); div.animate({width:'100px',opacity:'0.8'},"slow"); });
```
### 方法链接
类似回调函数
```
$("#p1").css("color","red") 
.slideUp(2000) 
.slideDown(2000);
```
## HTML
### DOM操作
text,html,val(设置或返回表单字段的值)
attr(获取属性值)，
val和attr区别：
1.val 可以获取手动输入的值，attr 不可以
2.用 val 赋值, val 可以获取值 , attr 不可以
3.用 attr 赋值, val 和 attr 都可以获取值, 如果手动改变输入的值，val 可以获取最新的值，attr 读到的还是 attr 一开始赋的值
#### 补充 元素的value
-   对于 "button"、"reset"、"submit" 类型 - 定义按钮上的文本
-   对于 "text"、"password"、"hidden" 类型 - 定义输入字段的初始（默认）值
-   对于 "checkbox"、"radio"、"image" 类型 - 定义与 input 元素相关的值，当提交表单时该值会发送到表单的 action URL。