---
title: css面试题
---
# 元素居中
以下container均为父元素
flex布局：
```css
.container {
   display: flex;
   justify-content: center; /* 水平居中 */
   align-items: center;     /* 垂直居中 */
}
```
grid布局：
```css
.container {
  display: grid;
  place-items: center; /* 水平和垂直居中 */
}
```
定位属性：
```css
.container {
   position: absolute;
   top: 50%;
   left: 50%;
   transform: translate(-50%, -50%);
}
```
calc()函数：
```css
.container {
   width: 300px;
   margin-left: calc(50% - 150px);
   margin-right: calc(50% - 150px);
   /* background-color: blue; */
}
```
特别地，对行内元素，可以设置``text-align:center``或者``width:fit-content``实现水平居中
垂直居中可以设置``line-height``和容器同高
```html
<div class="box">
    <span class="child">content</span>
</div>
<style>
    .box{
        height:200px;
        line-height:200px;
        background-color:red;
    }
</style>
```
对块级元素，可以设置``margin:0 auto``
# display有哪些属性
外部表现类（display-outside）：block、inline。
内部表现类（display-inside）：flex、grid、table、flow、flow-root、ruby。
列表元素类（display-listitem）：list-item。
内部结构类（display-internal）：table-row、table-cell、table-column、table-caption、table-row-group、table-header-group、table-footer-group、table-column-group、ruby-base、ruby-text、ruby-base-container、ruby-text-container。
元素显示类（display-box）：none、contents。
预组合类（display-legacy）：inline-block、inline-table、inline-flex、inline-grid。
