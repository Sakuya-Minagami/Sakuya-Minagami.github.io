---
title: flex布局
---
[flex布局](https://www.bilibili.com/video/BV1N54y1i7dG)
详细的b站教程，来来回回还是觉得有必要补一下布局基础
flex布局的核心在于主轴与侧轴的规划，规定元素按照轴向的排布方式
flex布局一般适用于移动端开发，适配页面
# 父元素设置
```html
<style>
        div{
            width: 80%;
            height: 300px;
            background-color: pink;

            /* display可以自适应浏览器 */
            /* flex布局给父盒子，子元素float，clear，vertical-align失效 */
            /* 默认横向排列，也可以竖向 */

            display: flex;
            
            /* 默认主轴为x */
            /* flex-direction: row; */
            /* 从右向左排列 属于翻转 */
            /* flex-direction: row-reverse; */
            /* flex-direction: column; */


            /* 默认 */
            /* justify-content: start; */
            /* 靠右排列 属于顺序排列 */
            /* justify-content: flex-end; */
            /* 主轴居中对齐 */
            /* justify-content: center; */
            /* 平分剩余空间 */
            /* justify-content: space-around; */
            /* 两边贴边再平分 */
            /* justify-content: space-between; */


            /* flex默认不换行 */
            /* flex-wrap: nowrap; */
            /* flex-wrap: wrap; */


            /* 设置侧轴子元素排列 */
            /* align-items: flex-start; */
            /* align-items: flex-end; */
            /* 拉伸 但子盒子不能给高度 侧轴拉满父盒子 */
            /* align-items: stretch; */


            /* 设置侧轴子元素多行排列方式 仅多行有效 */
            /* align-content: flex-start; */
            /* align-content: flex-end; */
            /* 可选项同justify-content */
            /* align-content: space-between; */


            /* direction和wrap复合属性 */
            /* flex-flow: row wrap; */

        }
        div span {
            width: 150px;
            height: 100px;
            background-color: purple;
            margin: 5px;
        }
    </style>

<body>
    <div>
        <span>1</span>
        <span>2</span>
        <span>3</span>
        <span>4</span>
        <span>5</span>
        <span>6</span>
    </div>
</body>
```
# 子元素设置
```html
<style>
        section {
            width: 60%;
            height: 150px;
            background-color: pink;
            margin: 0 auto;
            display: flex;
        }
        section div:nth-child(1) {
            width: 100px;
            height: 150px;
            background-color: red;
        }
        section div:nth-child(2) {
            /* 自适应剩下空间 左右已经固定的布局 */
            flex: 1;
            background-color: green;
        }
        section div:nth-child(3) {
            width: 100px;
            height: 150px;
            background-color: blue;
        }

        p {
            width: 60%;
            height: 150px;
            background-color: pink;
            margin: 0 auto;
            display: flex;
        }
        p span {
            /* 均匀分配布局 此时子元素没有规定宽度 直接按父元素三分之一分 */
            flex: 1;
        }
        p span:nth-child(2) {
            background-color: purple;
            /* 覆盖了上面的设置 */
            /* flex: 2; */

            /* 控制子元素单独布局 注意此时span缩成一行在底端*/
            /* align-self: flex-end; */

            /* 只用css调整排布顺序 默认值为0 */
            /* order: -1; */
        }
    </style>

<body>
    <section>
        <div></div>
        <div></div>
        <div></div>
    </section>
    <p>
        <span>1</span>
        <span>2</span>
        <span>3</span>
    </p>
</body>
```