---
title: vue响应式原理
---
[vuejs响应式基本原理 Vue.js 深入响应式原理](https://www.vue88.com/303.html)
[知乎：Vue源码分析基础之响应式原理](https://zhuanlan.zhihu.com/p/569928112)
# 概念
OBserver：为数据绑定getter和setter
Depend： 储存触发更新依赖的数组
Watcher：储存更新回调函数的数组
这三点可以称为响应式原理的三要素
# 基础：数据监听
vue2的方案
```js
  <div id="app">原始内容</div>
  <script>
  // 声明数据对象，模拟VUE实例data属性
  let data = {
	  msg: 'hello'
}
  // 模拟VUE实例的对象
  let vm = {}
  Object.defineProperty(vm, 'msg', {
	  enumerable: true,
	  configurable: true,
	  get () {
	  console.log('访问了属性')
	  return data.msg
},
  set (newValue) {
	  data.msg = newValue
	  document.querySelector('#app').textContent = data.msg
}
})
  </script>
```
