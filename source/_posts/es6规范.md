---
title: es6规范
---
复习用，倾向于点到为止，毕竟这个不需要特别熟，现用现查
所以清楚考点并回答也很重要
主要用阮一峰老师的博客，真神
# 字符串函数
[es5与es6字符串函数](https://blog.51cto.com/u_14082075/5465646#3_padStartpadEnd_327)
## startsWith()
确认字符串是否以指定字符开头，识别大小写
第二参数可选，默认0
```js
var str = 'Welcome to srcmini :)'; 
console.log(str.startsWith('Wel', 0));// true
console.log(str.startsWith('wel', 0));// false
```
## EndsWith()
同理，第二参数可选，默认全长
```js
var str = "Welcome to srcmini.";
console.log(str.endsWith("to", 10))// true
console.log(str.endsWith("To", 10))// false
```
## include()
确认字符串是否包含指定字符串
第二参数可选，指定开始位置，默认0
```js
let str = "hello world"

console.log(str.includes('world', 5));// true
console.log(str.includes('World', 11));// false
```
## repeat()
构建新字符串，重复并首位相连
```js
var str = "hello world";
console.log(str.repeat(5));
// hello worldhello worldhello worldhello worldhello world
```
## padStart()、padEnd()
```js
'x'.padStart(5, 'ab') // 'ababx'
'x'.padStart(4, 'ab') // 'abax'

'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'
```
## trimStart()、trimEnd()
消除空格
```js
const s = '  abc  ';

s.trim()   // "abc"
s.trimStart()   // "abc  "
s.trimEnd()   // "  abc"
```
# es5字符串函数
也补一下
## 常用字符串函数
### splice
参数指定起始，个数，以及填充
```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.splice(2,0,"Lemon","Kiwi");
// Banana,Orange,Lemon,Kiwi,Apple,Mango
```
### shift,unshift
在数组头移出，或者加入
### slice
指定起始和结束
```js
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
var citrus = fruits.slice(1,3);
// Orange,Lemon
```
## indexOf()、lastIndexOf()
查找字符位置
```js
let str = "abcdefg";
console.log(str.indexOf("a"));  // 0
console.log(str.indexOf("z"));  // -1 表示不存在

let str = "abcabc";
console.log(str.lastIndexOf("a"));  // 3
console.log(str.lastIndexOf("z"));  // -1
```
## concat()
连接字符串
```js
let str = "abc";
console.log(str.concat("efg")); //"abcefg"
console.log(str.concat("efg","hijk")); //"abcefghijk"
```
## split()
分割为字符串数组，第二参数可选，表示返回数组最大长度
```js
let str = "abcdef";
console.log(str.split("c")); // ["ab", "def"]
console.log(str.split(""));  // ["a", "b", "c", "d", "e", "f"]
```
## substr()、substring()、slice()
截取字符串
提取并返回字符串片段slice
```js
let str = "abcdef";
console.log(str.slice(1,6)); //"bcdef" [1,6)
console.log(str.slice(1));   //"bcdefg" 
console.log(str.slice());    //"abcdefg" 
console.log(str.slice(-2));  //"fg"
console.log(str.slice(6, 1));  //""
```
从指定位置提取substr
```js
let str = "abcdef";
console.log(str.substr(1,6));  //"bcdefg" 6代表切割的length
console.log(str.substr(1));   //"bcdefg" [1,str.length-1]
console.log(str.substr());  //"abcdefg" [0,str.length-1]
console.log(str.substr(-1));  //"g"
```
指定首位提取substring
```js
let str = "abcdef";
console.log(str.substring(1,6)); //"bcdef" [1,6)
console.log(str.substring(1));  //"bcdefg" [1,str.length-1]
console.log(str.substring()); //"abcdefg" [0,str.length-1]
console.log(str.substring(6,1)); //"bcdef" [1,6)
console.log(str.substring(-1)); //"abcdefg"
```
## toLowerCase()，toUpperCase()
大小写转换
简单，不写
## replace()、match()、search()
匹配或者替换字符串
替换replace
```js
let str = "abcdef";
console.log(str.replace("c", "z")) // abzdef
```
匹配match，返回的是结果数组
```js
let str = "abcdef";
console.log(str.match("c")) 
// ["c", index: 2, input: "abcdef", groups: undefined]
```
搜索search
```js
let str = "abcdef";
console.log(str.search(/bcd/)) // 1
```
## padstart
字符串补齐
str.padStart(targetLength,string)
# 模块编程规范
[模块化编程规范](http://www.taodudu.cc/news/show-5455145.html?action=onClick)
考虑出一个新篇，但是现在简写就好
## es5
AMD-异步模块
`require([module], callback);`
CMD-同步模块
```js
// CMD
define(function(require, exports, module) {
	var a = require('./a')a.doSomething()
	// 此处略去 100 行
	var b = require('./b')
	// 依赖可以就近书写b.doSomething()// ... 
})
// AMD 默认推荐的是
define(['./a', './b'], function(a, b) { 
// 依赖必须一开始就写好a.doSomething()
// 此处略去 100 行b.doSomething()...
}) 
```
CommonJS 规范
更加常用，但是在前端浏览器不适用，在node使用
**浏览器不兼容CommonJS的根本原因，在于缺少四个Node.js环境变量。**
module，exports，require，global

用法
```js
// foo.js
module.exports = function(x) {console.log(x);
};// main.js
var foo = require("./foo");
foo("Hi");
```
