---
title: Promise源码
---
参考：
[Promise官网](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise#%E5%8F%82%E8%A7%81)
[手写promise源码](https://zhuanlan.zhihu.com/p/407883529)
[b站视频](https://www.bilibili.com/video/BV1ph411f7Th)
# 基础结构
定义基础的promise结构，分别有
promise状态
修改状态的resolve和reject方法
实现嵌套的then方法
```JavaScript
// MyPromise.js
// 定义成常量是为了复用且代码有提示
const PENDING = 'pending' // 等待
const FULFILLED = 'fulfilled' // 成功
const REJECTED = 'rejected' // 失败
// 定义一个构造函数
class MyPromise {
  constructor (exector) {
    // exector是一个执行器，进入会立即执行，并传入resolve和reject方法
    exector(this.resolve, this.reject)
  }

  // Promise状态属性，初始为等待
  status = PENDING
  value = undefined
  reason = undefined

  // 用箭头函数让this指向当前实例对象
  resolve = value => {
    if(this.status !== PENDING) return
    this.status = FULFILLED
    this.value = value
  }
  reject = reason => {
    if(this.status !== PENDING) return
    this.status = REJECTED
    this.reason = reason
  }

  // 此步必不可少，因为throw error时不会调用reject，需要捕获后手动调用
  try {
    executor(resolve,reject);
  } catch(e) {
    reject(e);
  }


  then (successCallback, failCallback) {
    if(this.status === FULFILLED) {
      successCallback(this.value)
    } else if (this.status === REJECTED) {
      failCallback(this.reason)
    }
  }
}

module.exports = MyPromise
```
# 异步逻辑：发布-订阅模式
在上一步中，我们完成了promise的基本结构，但是它仍然是一个同步逻辑
问题就体现在，当我们连续调用then方法时，then方法中没有处理正在等待的promise的方案，也就是处在pending状态的promise
所以我们依次收集成功和失败回调函数，等到异步操作完成再调用
```JavaScript
// MyPromise.js
constructor{
	...
	this.onFulfilledCallbacks = [];
    this.onRejectedCallback = [];

	const resolve = (value) => {
            if(this.state === PENDING){
                this.state = FULFILLED;
                this.value = value;
                // 发布
                this.onFulfilledCallbacks.forEach(fn => fn());
            }
        }
	const reject = (reason) => {
        if(this.state === PENDING){
            this.state = REJECTED;
            this.reason = reason;
            // 发布
            this.onRejectedCallback.forEach(fn => fn());
        }
    }
}
if(this.state === PENDING){
            // 订阅
            this.onFulfilledCallbacks.push(() => {
                onFulfilled(this.value);
            });
            this.onRejectedCallback.push(() => {
                onRejected(this.reason);
            })
        }
```
# 细节：Promise/A+规范
## 链式调用
链式调用的精髓在于返回实例this本身，因此我们在then方法中定义新的promise并返回
```JavaScript
then(){
	let promise2 = new MyPromise((resolve, reject) => {
		// 将原本then中所有状态判断装进去
	}
	return promise2
}
```
## 错误的传递
虽然promise会返回promise对象，但我们需要传递的是value或者reason
注意，throw error本身是不会被reject的，需要手动操作
```JavaScript
// then中的状态判断
if (this.state === FULFILLED) {
    try {
        let x = onFulfilled(this.value);
    } catch (e) {
        reject(e);
    }
}

if (this.state === REJECTED) {
	try {
        let x = onRejected(this.reason);
    } catch (e) {
        reject(e);
    }
}
```
## 参数的传递
但是，我们完成的只是then的链式调用，对于then将要执行的两个回调函数，它们的返回值仍未确定。因此我们要进一步封装，手动在两个回调里面安排返回值
如果then没有写回调函数，那么采用默认方案，返回一个promise，并传递从上面接收到的值
```JavaScript
then(onFulfilled, onRejected){
onFulfilled = typeof onFulfilled ==="function" ? onFulfilled : (value) => value;
onRejected = typeof onRejected ==="function" ? onRejected : reason => { throw reason };
// 不能直接返回reason
}
```
如果then写了回调函数，那么就对它做处理``resolvePromise()``
```JavaScript
if (this.state === FULFILLED) {
        try {
            let x = onFulfilled(this.value);
            resolvePromise(promise2, x, resolve, reject);
            // 确保在外面能拿到resolve和reject方法
        } catch (e) {
            reject(e);
        }
}
if (this.state === REJECTED) {
        try {
            let x = onRejected(this.reason);
            resolvePromise(promise2, x, resolve, reject);
        } catch (e) {
            reject(e);
        }
}
// PENGDING 同理
```
现在我们要对回调函数的返回值x做处理``resolvePromise(promise2, x, resolve, reject)``
promise2是then方法即将返回的实例，x是上个Promise的返回值，
```JavaScript
function resolvePromise(promise2, x, resolve, reject) {
    if (promise2 === x) {
        return reject(new TypeError('Chaining cycle detected for promise #<MyPromise>'))
        // 循环调用，爆栈
    }
}
```
我们要进一步判定x是否有then方法，先判定是否是一个对象或者函数 *，也就是所谓的thenable*
```JavaScript
let called = false;

if ((typeof x === "object" && x !== null) || typeof x === "function") {
	if (typeof then === "function") {// Promise必须有then方法
				let then = x.then;
				// 修改this指向这个实例x
                then.call(x,(y) => {
                    if(called) return;// 避免重复调用成功或失败函数
                    called = true;
                    // 递归解决，因为y仍可能是Promise
                    resolvePromise(promise2,y,resolve,reject);
                },(r) => {
                    if(called) return;
                    called = true;

                    reject(r);// r肯定不是Promise不需要递归
                })
    } else {
	        resolve(x);// x只是个对象，返回
	       }
} else {
    resolve(x);// x只是个普通值，返回
}
```
根据文档要求，确认x是一个Promise后需要try和catch捕获异常
```JavaScript
 try {
	let then = x.then;// 这里可能会throw error 因为可能会被劫持 需要catch

    if (typeof then === "function") {
	    ...
    } else {
        resolve(x);
    }
} catch (e){
    if(called) return;
    called = true;

    reject(e);// 文档要求
}
```
## catch方法补充
复用then方法即可
```JavaScript
class MyPromise {
	...
	catch (errorCallback) {
        return this.then(null,errorCallback);
    }
}
```