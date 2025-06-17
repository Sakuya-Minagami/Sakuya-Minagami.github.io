---
title: react基础复习
---

# 脚手架安装
```shell
react-create-app 项目名
```
# 关于jsx
```jsx
// js写法
const div = React.createElement('div',null,span)
// jsx写法
const App = <div>
	<span>
		一个标签
	</span>
</div>

ReactDOM.render(App,document.getElementById("root"))
```
这样写不太规范，最好用圆括号包起来html内容
## 渲染方式
1.数据渲染 可以用大括号引用定义在函数体内的变量
2.属性绑定 相当于动态属性赋值，还是用大括号访问变量
3.列表渲染 记得使用map，return回来html标签即可
```jsx
const App = function(){
	const msg = "qwq"
	const list= ['1','2']
	return (
		// 数据渲染
		<h1>{msg}</h1>
		// 属性绑定
		<h1 title={msg}></h1>
		// 列表渲染
		{
			list.map(item => {
				return (
					<p>item</p>
				)
			})
		}
	)
}
```
# 文件结构
大部分都很常规，主要是讲一下封装的内容
源文件src/index.js
```js
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);

```
react和reactdom作为基本配置，APP作为根组件入口
根组件的渲染依靠render函数，传入组件名即可
再看根组件入口APP.js
```js
function App() {
  return (
    <div className="App">
		// ...
    </div>
  );
}

export default App;
```
跟常规的jsx没啥区别，也就是说从这里可以开始组件分层了
# 组件创建
## 函数组件
约定函数名以大写字母开头，返回值作为渲染内容
注意渲染时候要加标签
常用，不复习了
```
ReactDOM.render(<App />,document.getElementById("root"))
```
## 类组件
用es6语法class
约定以大写字母开头，必须提供render方法，返回值作为渲染
渲染时不加标签
```jsx
class App extends React.Component{
	render(){
		return (
			<div></div>
		)
	}
}
ReactDOM.render(App,document.getElementById("root"))

```
# 事件绑定
```
import React,{Component} from "react";

export default class Bind extends Component {
	state = {
		msg:"hello"
	}
	handleClick(e){
        console.log(this.state.msg)
    }
}
```
1.不能在render中直接访问this，即整个组件对象
```
handleClick(e){
        console.log(this.state.msg)
}

<button onClick={this.handleClick}>访问不到this</button>
```
2.可以通过bind修改this指向
```
<button onClick={this.handleClick.bind(this)}>修改this指向可以访问</button>
```
3.可以使用箭头函数，就没有this问题，但是注意必须带括号
```
<button onClick={() => this.handleClick()}>箭头函数不需要this</button>
```
4.也可以直接修改handleClick函数
```
handleClick = () => {
    console.log(this.state.msg)
}
```
