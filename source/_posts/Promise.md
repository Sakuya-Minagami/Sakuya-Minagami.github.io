---
title: Promise
---

# 引入
```JavaScript
setTimeout(function () {
    console.log("First");
    setTimeout(function () {
        console.log("Second");
        setTimeout(function () {
            console.log("Third");
        }, 3000);
    }, 4000);
}, 1000);
```
在执行异步操作的时候，可能因为层层嵌套导致形成多层缩进的回调地狱
但是实际上，我们执行的这些操作可能是同级的，只是逻辑导致了这种嵌套结构，所以我们需要借助链式操作，优化编程的结构
```JavaScript
//先实例化一个Promise
var pro=new Promise(function(resolve,reject){
  var name="李";
  resolve(name);//将name传递下去
});

pro.then(function(data){
  //输出成功传来的数据
  console.log(data);
  //先处理数据
  var newData1=data+"可";
  //将处理后的数据传递下去
  return Promise.resolve(newData1);
}).then(function(data){
  console.log(data);
  var newData2=data+"可";
  return Promise.resolve(newData2);
}).then(function(data){
   //输出最终的数据
   console.log(data);
   //故意抛出一个异常
   throw "Promise不想和你说话，并向你抛出了一个异常！！"
}).catch(function(mes){
   console.log(mes);
});
/* 输出结果如下 */
//李
//李可
//李可可
//Promise不想和你说话，并向你抛出了一个异常！！
```
这样的结构显然更优美些，对promise最基本的使用要求也同上

# 构造
```
new Promise(function (resolve, reject) {
    // todo...
});
```
最基本最基础的需求，就是实现异步操作的结构，
有一个操作作为Promise实例的传入函数，执行成功的操作作为resolve，以及一个失败的操作rejec
# 实例属性
对Promise实例，有两个属性：
PromiseState：最初是 pending，
resolve 被调用的时候变为 resolved，或者 reject 被调用时会变为 rejected。
PromiseResult：最初是 undefined，
resolve(value) 被调用时变为 value，或者在 reject(error) 被调用时变为 error。

不过一般不需要来直接访问promise的状态，而是根据执行的异步操作判断
*事实上，要避免通过访问promise状态来执行外部操作，这样才能将同步操作和异步操作充分隔离开*
# 实例方法
## resolve与reject方法
```JavaScript
new Promise((resolve, reject) => {
    if(result) {
        resolve()// resolve表示成功，传入操作成功的获取值
    } else {
        reject()// reject表示失败，传入error
    }
})
.then(() => {})
.catch(() => {})
```
## then
一般来说，写代码逻辑都会是遵从操作成功（resolve）后执行下一步（then）的；如果是new Promise是火车头，那么then就是一节节火车厢：
```JavaScript
var pro=new Promise(function(resolve,reject){
  var name="李";
  resolve(name);
});

pro.then(function(data){
  var newData1=data+"可";// 处理数据
  return Promise.resolve(newData1);// 传递数据
}).then(function(data){
  var newData2=data+"可";// 处理数据
  return Promise.resolve(newData2);// 传递数据
})
```
调用promise实例的then方法，它就等同于再次执行异步操作，同样传入两个函数，分别是成功回调和失败回调。后者不是必须的。
可以看到，then方法其实跟起始的promise函数结构非常相似，这也符合有时序和逻辑同级的特点
所以通常来说，我们都是``new Promise({}).then.then...``一直写成功时候的逻辑
**但是要注意，then一定要返回new Promise，否则执行的是同步操作！**
给出一个Promise调用ajax的例子
```JavaScript
const getJSON = function(url) {
  const promise = new Promise(function(resolve, reject){
    const handler = function() {
      if (this.readyState !== 4) {
        return;
      }
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };
    const client = new XMLHttpRequest();
    client.open("GET", url);
    client.onreadystatechange = handler;
    client.responseType = "json";
    client.setRequestHeader("Accept", "application/json");
    client.send();

  });

  return promise;
};

getJSON("/posts.json").then(function(json) {
  console.log('Contents: ' + json);
}, function(error) {
  console.error('出错了', error);
});
```
## catch
我们完成了在不出任何意外的前提下完成异步操作，但我们必须面对出意外的情况，也就是promise在一步步resolved过程中，出现了rejected的情况
实际上，这样的情况很少见，除了主动在上级promise使用reject方法以外，就是error和throw的情况
```JavaScript
let p1 = Promise.reject('foo');// 创建一个rejected状态实例

let p10 = p1.then(null,() => { throw 'baz'; } );
// 抛出异常会reject
// 报错: Uncaught (in promise) baz
// p10:Promise <rejected>: baz

let p11 = p1.then(null, () => Error('qux'));
// 抛出错误不会reject
// p11: Promise <resolved>:qux
```
而``p.catch(onRejectd)``就是统一地用来处理rejected的回调函数，它和``p.then(null,onRejected)``是等价的

*未完待续*