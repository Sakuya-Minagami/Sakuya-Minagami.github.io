---
title: element-ui
---
虽然想着自己写也差不多，但是
一呢，可以偷懒
二呢，别人常用的组件库，要说自己也不会，多尴尬
顺便，element-ui plus好像用的不多，还是用原装吧
# 配置
## 安装/引入
```
npm i element-ui -S
```
或者
```
<!-- 引入样式 --> 
<link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css"> 
<!-- 引入组件库 --> 
<script src="https://unpkg.com/element-ui/lib/index.js"></script>
```
## 使用
配置main.js
```
// main.js
import Vue from 'vue'; 
import ElementUI from 'element-ui'; 
import 'element-ui/lib/theme-chalk/index.css'; 
import App from './App.vue'; 
Vue.use(ElementUI); 
new Vue({ el: '#app', render: h => h(App) });
```
注意：顺序最好不要弄乱，否则可能报错组件未定义
## 按需引入
```
npm install babel-plugin-component -D
```
babelrc配置
```
{ "presets": [["es2015", { "modules": false }]], 
"plugins": [ [ "component", 
{ "libraryName": "element-ui", 
"styleLibraryName": "theme-chalk" } ] ] }
```
main.js配置
```
import Vue from 'vue'; 
import { Button, Select } from 'element-ui'; 
import App from './App.vue'; 
Vue.component(Button.name, Button); 
Vue.component(Select.name, Select); /* 或写为 * Vue.use(Button) * Vue.use(Select) */ new Vue({ el: '#app', render: h => h(App) });
```
