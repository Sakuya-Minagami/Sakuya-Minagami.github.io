---
title: Getters & Setter与响应式原理
---
getter和setter是对象的一种特殊属性
```js
const obj = {
	arr: ['a','b','c'],
	get test(){
		return this.arr[0]
	}
}
console.log(obj.test)
// obj.test()是错的
```
注意，get是修饰符，修饰一个函数
但是调用时，必须将函数当成属性，不能带括号
**它的作用是每次访问对象的get修饰属性时，执行对应回调函数**

同理，setter类似
```js
const language = {
	set current(name) {
		this.log.push(name);
	},
	log: []
};
language.current = 'EN';
language.current = 'CN';
console.log(language.log);
// 输出Array['EN','CN']
```
**setter的作用是每次修改对象的set修饰属性时，执行对应回调函数**
*set与get的返回值都由回调函数自己决定*
# 例子
[JS中的OBject.defineProperty](https://blog.51cto.com/ahuiok/5895003)
```js
let number = 18;
let person = {
	name: 'Jack',
	gender: 'male'
}

Object.defineProperty(person,'age',{
	get: function(){
		console.log("age属性被读取了");
		return number;
	}
})
console.log(person)
```
执行后，控制台输出person，但是要点开``age(...)``，控制台才输出“age属性被读取”

# Object.defineProperty
``Object.defineProperty(obj,prop,descriptor)``
该方法用于直接对对象obj添加属性，可选有：
-- value， 是当前属性的值，可为任意有效JS值；默认值：undefined;
-- writable，为 true 时，当前属性的 value 才能被修改；默认值：false；
-- get，属性的 getter 函数，不传参，会传入 this 对象，函数返回值是当前属性的值；默认值：undefined;
-- set， 属性的 setter 函数，调用此函数修改当前属性的值，接受参数，会传入赋值时的 this 对象；
*特别地，用等号=声明的属性是深色的，defineProperty声明的是浅色的*
**用defineProperty声明的属性不可修改，不可遍历，不可删除**
如果用writeable、enumerable、configuralbe:true初始化声明，就和=声明一样了

## 用法
``Object.preventExtensions()``禁止一个对象添加属性--禁扩
``Object.seal()``禁止添加并不可配置--密封
``Object.freeze()``禁止添加删除修改配置--冻结
# vue3响应式原理：捕获器
在Vue2.x中响应式原理实现的核心就是使用的``Object.defineProperty``，而在Vue3.x中响应式原理的核心被换成了Proxy
``Object.defineProperty``有三个缺点：
当对象属性层次较深时，需要深度监听，计算量大
当数组增改删元素时无法监听，需要使用``vue.$set``和``vue.$delete``辅助监听
需用重写数组原生方法来监听数组
```js
  <div id="app">原始内容</div>
  <script>
  const data = {
  msg: 'hello',
  content: 'world',
  arr: [1, 2, 3],
  obj: {
  name: 'will',
  age: 18
      }
    }


  const vm = new Proxy(data, {
  get (target, key) {
  return target[key]
      },
  set (target, key, newValue) {
  //数据更新
  target[key] = newValue
  //视图更新
  document.querySelector('#app').textContent = target[key]
      }
    })
  </script>
```
