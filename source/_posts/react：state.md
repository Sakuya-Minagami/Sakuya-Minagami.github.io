---
title: react：state
---
发现state的内容特别多，还是专门开一篇来讲
这一篇讲得非常好[知乎：深入浅出state原理](https://zhuanlan.zhihu.com/p/470429343)

个人理解
类组件是由构造函数实例化来的，它是一个实例对象，有属性和修改自身的方法
函数组件是一个函数，可以引入参数，但是更新时会重新执行，导致state被初始化覆盖
**值得一提的是，执行顺序与视图更新流程是两码事，不要弄混**

# 使用
第一个参数为参数形式updater，也可直接接受对象
第二个参数为更新后回调
```js
setState(updater, [callback])

// 参数形式updater 也可直接接受对象
(state, props) => stateChange
// 例如
// this.setState((state, props) => {
//   return {counter: state.counter + props.step};
// });
```
## 补充：比起componentDidUpdate有什么优势？
逻辑一致
批量更新
# 批量更新问题
原话

`setState()` 将对组件 state 的更新排入队列，并通知 React 需要使用更新后的 state 重新渲染此组件及其子组件。这是用于更新用户界面以响应事件处理器和处理服务器数据的主要方式  
为了更好的感知性能，React 会延迟调用它，然后通过一次传递更新多个组件。React 并不会保证 state 的变更会立即生效  
`setState()` 并不总是立即更新组件。它会批量推迟更新。这使得在调用 `setState()` 后立即读取 `this.state` 成为了隐患。为了消除隐患，请使用 `componentDidUpdate` 或者 `setState` 的回调函数（`setState(updater, callback)`），这两种方式都可以保证在应用更新后触发  
除非 `shouldComponentUpdate()` 返回 `false`，否则 `setState()` 将始终执行重新渲染操作。如果可变对象被使用，且无法在 `shouldComponentUpdate()` 中实现条件渲染，那么仅在新旧状态不一致调用 `setState()`可以避免不必要的重新渲染
# 视图更新问题
直接修改this.state.qwq，与采用setState不同点在于，后者还多了向react触发视图更新
触发setState后的生命周期是
*static getDerivedStateFromProps*
shouldComponentUpdate
render
getSnapshotBeforeUpdate
componentDidUpdate
*对于数组操作，可以用...解构出来，直接用push肯定不行*
# 同步异步与重复调用问题
react17以前版本
setState异步更新
```js
this.setState({ count: this.state.count + 1})
console.log(this.state.count)

this.setState({ count: this.state.count + 1})
console.log(this.state.count)

setTimeout(() => {
	console.log(this.state.count)
	
	this.setState({ count: this.state.count + 1})
	console.log(this.state.count)
	
	this.setState({ count: this.state.count + 1})
	console.log(this.state.count)
})
// 00123
```
为什么要采用异步更新？
因为setState是要触发更新的生命周期的，为了节约性能就在各种处理完成之后再进行渲染，也就是异步
# 批量更新解决方案：传入函数
此时就不会出现合并的问题了，会实打实jy+3
```js
function increment(state, props) {
  return {jy: state.jy + 1};
}

function incrementMultiple() {
  this.setState(increment);
  this.setState(increment);
  this.setState(increment);
}
```
*混用函数与对象的传参会触发合并*
# 数据更新后再访问问题
```js
onHandleClick() {
    this.setState(
        {
            count: this.state.count + 1,
        },
        () => {
            console.log("点击之后的回调", this.state.count); // 最新值
        }
    );
}
```
两种办法，显然后者在数据更新前调用
```js
this.setState(state => {
 console.log("函数模式", state.count);
 return { count: state.count + 1 };
});
```
# 一个有点奇葩又有点意思的问题
[setState是宏任务还是微任务](https://segmentfault.com/a/1190000040445026)
结论：react靠自己的流程实现的setState异步操作，本质上同步，不属于原生js定义的异步操作
