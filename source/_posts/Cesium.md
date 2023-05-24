---
title: Cesium
---

# 安装
```
npm install cesium
```
由官网下载仓库``https://github.com/CesiumGS/cesium.git``
```
npm install
npm run build
```
# 文件目录
针对官网下载的源码
## Apps


# 一点任务
## 搞定卫星轨迹
强调一个知识点，模型要放在public下，表示用绝对路径访问
## 完成vue配置
思路1 webpack 在vue.config引入依赖
思路2 直接在src下打包cesium使用
思路3 VueCesium 一个库，相当于组件库
思路4 vue-cesium-plugin 一个插件 自动配置

## 最终方案 vite-plugin
1.vite创建vue项目
```
yarn create vite-app vue-cesium
yarn add -D cesium vite vite-plugin-cesium
yarn install
```
2.配置
创建vite.config.js
```js
import { defineConfig } from 'vite';
import cesium from 'vite-plugin-cesium'; 
import vue from '@vitejs/plugin-vue'

export default defineConfig({
    plugins: [cesium(),vue()],
});
```
3.简单应用
App.vue下
```vue
<template>
  <div id="cesiumContainer"></div>
</template>

<script>
import * as Cesium from 'cesium';

export default {
  mounted(){
    const viewer = new Cesium.Viewer('cesiumContainer');
  }
}
</script>
```
# 关于后端数据的打算
需要加载卫星模型，轨道模型，都可以通过CZML文件加载
那就暂时把重点放在其前端功能上，就用现有CZML展示可视化数据
比如标签什么的
# 这次的项目经历回顾
提供：卫星两行轨道数据TLE，要求绘制卫星及轨道
卫星需要圆锥跟随，两个以上卫星时，卫星靠近绘制连线
在找到的代码示例中，根据TLE计算出了CZML，提供给卫星
但是圆锥的高度需要调整，卫星高度和圆锥高度不一致
代码中重新封装了一个SampleProperty对象给圆锥，然后再自己调用
## 圆锥消失
初始化时有，但是点时间轴之后圆锥消失
现在来看，封装的属性可能失效了

## 圆锥对应time和卫星不一致
看得出圆锥在卫星附近的地方
