---
title: musci-框架
---
# 整体布局-html
## 视图：
路由组件，下端音乐播放组件（footmusic）固定
注意使用overflow：hidden，轮播组件会溢出界面，否则页面会侧面大量空白
### 路由组件：
主界面（homeview），歌单界面（itemmusiclist）
#### 主界面：
顶部导航（topnav），顶部轮播（swipertop），符号列表（iconlist），歌单列表（musiclist）
##### 顶部导航
只显示发现部分，加粗
布局为space-around，两个贴边，四个布局中间并均分
##### 顶部轮播
使用轮播组件，设定图片大小即可撑开容器
##### 符号列表
##### 歌单列表
使用轮播，注意需要限制容器宽度，否则拖不动
a标签要连着图片和简介

重点：playcount的定位
