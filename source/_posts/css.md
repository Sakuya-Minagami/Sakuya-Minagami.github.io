---
title: css学习
---
顶不住了，css内容真多
接下来更新每日css吧
### 8-18 
## 水平方向布局
盒子有七个元素，行像素和为800
margin-left/right
border-left/right
padding-left/right
width
其中margin和width可设置auto

1.无auto时，强制改变margin-right
2.width和一个margin设置auto，width默认最大，设置的margin+=0
3.均auto，width最大，margin=0
4.margin均设置auto，宽度固定，margin相等，此时居中

特别地，设置水平居中：
```
width: 100px;
margin: 0 auto;
//前者表示上下，后者表示左右
```
## 行内元素盒模型
行内元素不支持设置宽高
padding，border，margin水平有效，垂直无效
display属性：设置元素类型：inline，block，inline-block，table，none
行内块可以设置宽高，不会独占一行；尽量不用

### 8-30
[cale](https://www.cnblogs.com/shunk/p/10690757.html)

在写跷跷板时候发现球怎么都不动，从评论知道是cale函数出了问题
```
left: calc(100% - 20px);
```
原来空格不能省略，平时写代码偶尔也会忽略空格。。

cale是css3的属性，可以用来计算元素的长度属性，表达式：
```
 width: calc(expression);

```
用calc可以解决元素宽度计算问题，防止溢出容器
```
.demo {  
width: 300px;  
background: #60f;  
padding: 3px 0;  
}  
.box {  
background: #f60;  
height: 50px;  
padding: 10px;  
border: 5px solid green;  
width: 90%;/_写给不支持calc()的浏览器_/  
width:-moz-calc(100% - (10px + 5px) * 2);  
width:-webkit-calc(100% - (10px + 5px) * 2);  
width: calc(100% - (10px + 5px) * 2);  
}
```
其中有的元素使用百分比，有的元素使用px，但是在calc中可以直接加入计算
支持百分比，em，px，rem

### 9-1
## vue的滑动导航
千锋视频路上顺便学过来的，当时没有仔细挖深，后来应用了才知道很多学问
1.将导航用transition组件包起来，自定义name
```
<transition name="navbar">

            <navbar v-show="navbarisShow" class="navbar"></navbar>

</transition>
```
2.在css中写进入和离开，注意命名
```
.navbar-enter-active{

        animation: slide 1s;

}

.navbar-leave-active{

        animation: slide 1s reverse;

}
```
完整选项
-   进入的样式
    -   `xxx-enter`，进入的起点
    -   `xxx-enter-to`，进入的终点
    -   `xxx-enter-active`，进入的过程
-   离开的样式
    -   `xxx-leave`，离开的起点
    -   `xxx-leave-to`，离开的终点
    -   `xxx-leave-active`，离开的过程

### 9-2
## position 指定元素定位类型
候选值：
static,relative,fixed,absolute,sticky
static:默认值
fixed:相对窗口固定
relative:相对正常位置的偏移
```
h2.pos_left { position:relative; left:-20px; } 
h2.pos_right { position:relative; left:20px; }
```
absolute:相对最近已定位父元素，若无定位html
sticky:滚动超出窗口时停留

特别地，可用z-index确定元素覆盖关系
```
img { position:absolute; left:0px; top:0px; z-index:-1; }
```
## background-size
候选值：px px | % % | cover | contain
cover表示全覆盖，contain表示等比例放大

顺便，必要时候先html{height:100%}，才能让body继承整个浏览器大小

### 9-3
## float
css三种布局方式：普通流（标准流），浮动，定位
普通流块级元素占一行，由上到下；行内元素从左到右

对于多个设置了浮动的元素，会同行对齐显示，无缝隙
<b>浮动元素具有行内块特性</b>
-   块级盒子：没有设置宽度时默认宽度和父级一样宽，但是添加浮动后，它的大小根据内容来决定
-   行内盒子：宽度默认和内容一样宽，直接设置高宽无效，但是添加浮动后，它的大小可以直接设置
-   浮动的盒子中间是没有缝隙的，是紧挨着一起的
-   即：默认宽度由内容决定，同时支持指定高宽，盒子之间无空隙

### 9-4 
## border-radius
四个方向分别为，左上，右上，右下，左下
```
border-radius : 30px 30px 30px 30px;
border-radius: 30px;//等价

border-radius: 50px 0px;//左上右下50，右上左下0
```
用px表示圆弧，用border-radius: 50%;表示椭圆
## scale
表示缩放
（1）scaleX(x)：元素仅水平方向缩放（X轴缩放）；

（2）scaleY(y)：元素仅垂直方向缩放（Y轴缩放）；

（3）scale(x,y)：元素水平方向和垂直方向同时缩放（X轴和Y轴同时缩放）；
```
transform: scale(1.2,1);
```
## 关于jump的一点设计思路
一个正方形和一个椭圆形阴影
正方形顺时针旋转，动画只设计90度，右下角变圆再变方，并且y轴上下移动跳跃效果
椭圆随着x轴伸缩

