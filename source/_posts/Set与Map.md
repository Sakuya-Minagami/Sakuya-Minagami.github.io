---
title: Set与Map
---
es6新语法，数据结构
# Set
set是一种数据结构，不存在键值对，或者说键就是值本身
因此set数据结构本身就像个黑匣子，只能全倒出来看里面储存的内容
**Set唯一的优势是元素永不重复**
可以用来去重
## 初始化
set接受一个拥有迭代器接口的数据结构，直接传入基础数据类型会报错
```js
let set = new Set([1,2,2,1,4,3,5])
console.log(set)//Set(5) {1, 2, 4, 3, 5}
```
## 属性
仅有一个属性size
```js
let set = new Set([1,2,2,1,4,3,5])
console.log(set.size)//5
```
## 方法
Set.prototype.add(value) —— 添加某个值到 Set 的末尾，返回 Set 本身。
Set.prototype.delete(value) —— 删除某个值，返回布尔值，表示是否删除成功。
Set.prototype.has(value) —— 返回一个布尔值，表示该值是否为 Set 的元素。
Set.prototype.clear() —— 清除所有成员，没有返回值。

# Map
[浅析map和weakmap区别和使用场景](http://www.hackdig.com/04/hack-973869.htm)
map本质上是一个键值对的集合。和传统对象结构相比，传统的对象只能用字符串作为键名，这就在使用上造成了很大的限制了。这也是新增 Map 的原因之一。
## 使用场景
要添加的键值名和 Object 上的默认键值名冲突，又不想改名时，用 Map
需要 String 和 Symbol 以外的数据类型做键值时，用 Map
键值对很多，有需要计算数量时，用 Map
需要频繁增删键值对时，用 Map
## 初始化
map同样接受一个拥有迭代器接口的数据结构
可以依次入表
```js
const m = new Map();
const o = {p: 'Hello World'};
// 键为o 值为content
m.set(o, 'content')
m.get(o) // "content"
```
可以封装入表
注意传入的是多层数组，相当于将键和值合并为元素一齐储存
```js
const map = new Map([
  ['name', '张三'],
  ['title', 'Author']
]);
// 键为子数组前者 值为后者
map.size // 2
map.has('name') // true
map.get('name') // "张三"
```

```js
const map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

[...map.keys()]
// [1, 2, 3]

[...map.values()]
// ['one', 'two', 'three']

[...map.entries()]
// [[1,'one'], [2, 'two'], [3, 'three']]

[...map]
// [[1,'one'], [2, 'two'], [3, 'three']]
```
## 属性
同样是size属性，应该不需要再写了
## 方法
set、get、has、delete、clear和map同
## 使用
#### 和数组互换
map没有map和filter方法，但是有foreach方法
使用前两者时，需要扩展成数组
```js
const map0 = new Map()
  .set(1, 'a')
  .set(2, 'b')
  .set(3, 'c');

const map1 = new Map(
  [...map0].filter(([k, v]) => k < 3)
);
// 产生 Map 结构 {1 => 'a', 2 => 'b'}

const map2 = new Map(
  [...map0].map(([k, v]) => [k * 2, '_' + v])
    );
// 产生 Map 结构 {2 => '_a', 4 => '_b', 6 => '_c'}
```
#### 和对象互换
map的键是字符串则无损换，否则强制转字符串
```js
function strMapToObj(strMap) {
  let obj = Object.create(null);
  for (let [k,v] of strMap) {
    obj[k] = v;
  }
  return obj;
}

const myMap = new Map()
  .set('yes', true)
  .set('no', false);
strMapToObj(myMap)
// { yes: true, no: false }
```

```js
function objToStrMap(obj) {
  let strMap = new Map();
  for (let k of Object.keys(obj)) {
    strMap.set(k, obj[k]);
  }
  return strMap;
}

objToStrMap({yes: true, no: false})
// Map {"yes" => true, "no" => false}
```

# WeakMap
[知乎：weakmap](https://zhuanlan.zhihu.com/p/161520370)
WeakMap 提供了一种从外部扩展对象而不干扰垃圾收集的方法。
它是一个 Map 字典，其中的键很弱，也就是说，如果对该键的所有引用都丢失，并且不再有对该值的引用，则可以对该值进行垃圾回收。
键只能是对象，不移除键即不回收，造成内存泄漏
```js
const wm = new WeakMap();
wm.set({}, "val");// 没有任何引用，创建后即销毁
```
## 强引用与弱引用
[博客园](https://www.cnblogs.com/houxianzhou/p/16384815.html)
二者的区别是：
如果一个变量保存着对一个对象的强引用，那么这个对象将不会被垃圾回收，但是如果一个变量只保存着对这个对象的弱引用，那么这个对象将会被垃圾回收
```js
let obj = { name: 'toto' }
let mapObj = new Map()
mapObj.set(obj, 'any value')
// 强引用只要被引用就不销毁，无论是否被覆盖
obj = null
mapObj.size() // 1

let obj = { name: 'toto' }
let weakmapObj = new WeakMap()
weakmapObj.set(obj, 'any value')
// 弱引用不引用即销毁
obj = null
weakmapObj .size() // 0
```
**特别地，weakmap不能调用.keys() / .values() /.entries()，因为不知道何时被销毁**
## 应用
在各篇文章应用都比较统一：dom节点元数据、部署私有属性、数据缓存
dom数据：
```js
let wm = new WeakMap();
let element = document.querySelector(".element");
wm.set(element, "data");
 
let value = wm.get(elemet);
// dom销毁即销毁数据
element.parentNode.removeChild(element);
element = null;
```
私有变量：
*更多内容见变量与作用域*
```js
const privateData = new WeakMap();
class Person {
	constructor(name, age) {
		privateData.set(this,{ name: name, age: age });
	}
	getName() {
		 return privateData.get(this).name;
	}
	getAge() {
	    return privateData.get(this).age;
	}
}
export default Person;
```
数据缓存：
其实就跟算法题要求的保存数组信息类似
```js
const cache = new WeakMap();
function countOwnKeys(obj) {
  if (cache.has(obj)) {
    console.log('Cached');
    return cache.get(obj);
  } else {
    console.log('Computed');
    const count = Object.keys(obj).length;
    cache.set(obj, count);
    return count;
  }
}
```