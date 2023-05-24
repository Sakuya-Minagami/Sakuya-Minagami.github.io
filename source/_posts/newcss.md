---
title: 新的css笔记
---
原因是发现自己之前的多级标题一直用错了，直接换个新的开始写算了2333
# 9-4
## css伪类
[css伪类](https://m.runoob.com/css/css-pseudo-classes.html)
### anchor伪类
```
a:link {color:#FF0000;} /* 未访问的链接 */ 
a:visited {color:#00FF00;} /* 已访问的链接 */ 
a:hover {color:#FF00FF;} /* 鼠标划过链接 */ 
a:active {color:#0000FF;} /* 已选中的链接 */
```
### child 子元素
:first-child
:last-child
选择所有p元素的正序/反序子元素

:nth-child(n)
:nth-last-child(n)
选择所有p元素正序

:only-child
# 9-21
## justify-content属性
justify-content 用于设置或检索弹性盒子元素在主轴（横轴）方向上的对齐方式。

**提示：**使用 align-content 属性对齐交叉轴上的各项（垂直）。

/* 对齐方式 */  
justify-content: center;     /* 居中排列 */  
justify-content: start;      /* 从行首开始排列 */  
justify-content: end;        /* 从行尾开始排列 */  
justify-content: flex-start; /* 从行首起始位置开始排列 */  
justify-content: flex-end;   /* 从行尾位置开始排列 */  
justify-content: left;       /* 一个挨一个在对齐容器得左边缘 */  
justify-content: right;      /* 元素以容器右边缘为基准，一个挨着一个对齐, */  
  
/* 基线对齐 */  
justify-content: baseline;  
justify-content: first baseline;  
justify-content: last baseline;  
  
/* 分配弹性元素方式 */  
justify-content: space-between;  /* 均匀排列每个元素  
                                   首个元素放置于起点，末尾元素放置于终点 */  
justify-content: space-around;  /* 均匀排列每个元素  
                                   每个元素周围分配相同的空间 */  
justify-content: space-evenly;  /* 均匀排列每个元素  
                                   每个元素之间的间隔相等 */  
justify-content: stretch;       /* 均匀排列每个元素  
                                   'auto'-sized 的元素会被拉伸以适应容器的大小 */  
  
/* 溢出对齐方式 */  
justify-content: safe center;  
justify-content: unsafe center;  
  
/* 全局值 */  
justify-content: inherit;  
justify-content: initial;  
justify-content: unset;