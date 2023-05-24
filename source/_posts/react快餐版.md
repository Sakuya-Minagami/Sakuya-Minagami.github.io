---
title: react快餐版
---
要赶进度了，拼命学吧
# 语法
## 注意点
变量用{}框起来
html中for，class重写了方法
style需要{}在外，里面再写一层对象（再套一层大括号），记得驼峰命名

### for循环：
不能使用foreach，只能用map，因为只有后者有返回值
```
{//写js记得写大括号
	arr.map((item,index)=>{
		return <li key={index}>{item}</li>
	})
}
```
## 累加，setstate
react没有类似vue的数据绑定
```
state = {
	num: 1
}
render(){
return (
	<div>
		<h2>{{this.state.num}}<h2/>
		<button> onClick={()=>this.setState({num: this.state.num+1})}>累加<button/>
	<div/>
)
}
```
必须使用setState修改数据
待学：setState底层方法，与vue比较（面向数据，值得研究）
### state完整写法
```
//state完整写法
    constructor(props){
        super(props)
        this.state = {
            num: 1
        }
    }
```
## (类组件与)函数式组件
类组件基于es6 class

函数式组件三个特点
没有生命周期、没有this、没有state状态
## useState
```
const [count,setCount] = useState(initialCount)
```
count为变量名
setCount为函数，传入的参数为对count的修改值
initialCount为count初值
## useEffect
可以类似mounted效果：
```
useEffect(()=>{
	console.log("挂载完成")//可以在此处写ajax
})
```
可以类似updated效果：
```
useEffect(()=>{
	console.log("num1更新了")
},[])//将要监听的变量写在数组里，不写表示全监听，空数组表示不监听
```
可以类似beforedestoryed效果：
```
useEffect(()=>{
	return ()=>{
		console.log("销毁阶段")//延时替换组件即可销毁
	}
})
```
## 父子通信
### 父传子
```
//子组件
function Child(props){
	return <h2>子组件-{{props.num}}</h2>
}
//父组件
function Father(props){
	return <child num={props.num}/>
}
//爷爷组件
function grandFather(){
	return <Father num="123"/>
}
```
在父组件标签，冒号参数名
在子组件，传入参数props，为一个对象
### 子传父
将父组件的修改函数传给子组件，子组件调用即实现了修改父数据
### context 跨级通信
```
ipmort useContext from "react"
const NumContext = createContext()

//爷爷组件
	return (
		<NumContext.Provider value={{num,setNum}}>
			<Father/>
		<NumContext.provider/>
	)
//父组件
	return (
		<Childe/>
	)
//子组件
	return (
		<NumContext.Consumer>
		{
			({num,setNum}) => (
				<>
					<h2>{num}</h2>
					<button onClick={()=>setNum()}>修改num</button>
				</>
			)
		}
		<NumContext.Consumer/>
	)
```
注，别忘了标签里写js要套{}，参数和方法一起传就要写成对象，传一个对象再解析

特别地，使用useContext可以省的写子组件的consumer
```
import useContext from 'react'
const numContext = createContext()

function Child() {
	const {num, setNum} = useContext(NumContext)
}
```
即可在子组件中直接使用
## 受控组件与非受控组件
[博客园](https://www.cnblogs.com/sexintercourse/p/16082406.html)
[知乎](https://zhuanlan.zhihu.com/p/536322574)
### 受控组件
```
render: function() {
    return <input type="text" value="Hello!" />;
  }

```
此时value被锁定，无法修改
要修改就要维护一个状态state，在input内用onChange方法和setState方法修改
也就是说，设定了表单的value，就要跟着设定一个onchange方法去允许修改value
```
const inputChange = (e) => {
	console.log(e)//传入的e是事件对象
	setValue
}

...
return (
	<div>
		<input value={value} onChange={inputChange}/>
	</div>
)
```
### 非受控组件
[非受控组件-react官方文档](https://react.docschina.org/docs/uncontrolled-components.html)
```
import { useState,useRef } from 'react'

const element = useRef(null)

return(
	<input type="text" ref={element}/>
	//此时element被赋值,current为获得的input标签
	<button onClick={() => console.log(element.current.value)}>获得value</button>
)
```
## memo
父组件setState更新数据时会同时导致子组件更新引起浪费

memo是一个hook函数，传入一整个函数即可
可以缓存组件，不受父组件影响
```
import {memo} from "react"
const Child = memo(() => {
	console.log('子组件更新了')
	return <div>子组件</div>
})

...
return (
	<>
		{num}
		<button onClick={()=>setNum(num +1)}>累加</button>
		<Child>
	</>
)
```
如果不写memo，就会导致子组件child更新
但是memo只对静态子组件有效
```
//子组件 此时memo就会失效
return <button onClick="{props.setNum}"></button>
```
## useCallback
```
import useCallback
//父组件
const doSth = useCallback(()=>setNum(num => num+1),[])
//写空数组表示不检测任何数据
```
此时使用回调函数，memo又生效了

原理是useCallback返回一个函数，用一个变量（doSth）保存起来
useCallback第二个参数，数组表示对参数监听，参数变化即重新生成doSth
也就重新执行了doSth
因此它解决了memo的问题，同时也解决了num重新赋值问题
## useMemo
类似useCallback，不同的是在函数中写一层return之后再返回,称为高阶函数
```
useMemo(()=>{
	return () => setNum(num => num+1)
}
```
# react半场总结
能告诉我，你学到了什么吗？

首先是语言形式，第一次接触了jsx文件，了解到js和xml可以一起写
但是这样第一个问题就是会导致语言混乱，所以react重写了冲突的html方法，规定了写js和写标签的范围

其次是react的数据管理并不灵活，并不像vue有严格的数据动态绑定，data变化即引起页面刷新
react的数据都保存在state中，只有setState方法才能修改数据并同时刷新页面

相应地，在useEffect钩子函数中，也没有严格的更新来源的概念，只负责刷新页面执行回调函数
不像vue对数据更新流程有严格划分，react通过区分对指定参数的监听完成不同钩子函数的任务

接下来是组件方面，父子通信方面没有明显差异，context跨级通信也和vue bus总线类似
重点是出现了受控组件与非受控组件概念
其实这方面学得不太清楚，只知道前者需要设定value时还要连同绑定onChange方法修改value
后者接近一个不需要管理的dom节点，可以用useRef读value值

再然后是memo钩子函数，可以缓存子组件，避免在父组件setState时直接导致子组件更新
但是如果子组件调用了传来的父组件函数，子组件仍会重新渲染
这时就可以使用useCallback钩子函数，在第二个参数规定要监听的数据，将父组件要传给子组件的方法用useCallback钩子函数传过去，就可以在指定时候更新子组件了
useMemo钩子函数，看起来像memo和useCallback的结合体？找了找发现不出特别的用法
# redux
[redux简单用法](https://blog.csdn.net/weixin_58726419/article/details/121321895)
[redux详解](https://blog.csdn.net/weixin_57218747/article/details/118070930)
## AndDesign
记得引入时候使用
```
import 'antd/dist/antd.min.css'//不用min会报错
```
# react router
[react router  v5&6](https://blog.csdn.net/qq_45799465/article/details/124257523)
记得查看react-router版本，安装最新版v6
`` npm install -S react-router-dom@6``
## 配置
在根目录index.js中
``import Router from './router/index.jsx'``
使用Router组件``<Router/>``为入口

在根目录router/index.jsx中配置所有的组件和路径
```JavaScript React
import {BrowserRouter, Routes, Route} from 'react-router-dom'

const BaseRouter = () =>(
<BrowserRouter>
	<Routes>
		<Route path='/' element={<App/>}>
			//App为根组件
			//在此处写嵌套路由
		</Route>
	</Routes>
</BrowserRouter>
)
```
也就是说将BaseRouter作为根组件传入index

注，BrowserRouter 为history模式，HashRouter为hash模式
## 路由跳转方式
``<Link to='/home?id=qwqq'>首页</Link>``
1.声明式导航，可携带参数，类似router-link
```JavaScript React
import { Outlet,Link,useLocation,useNavigate } from "react-router-dom";

...//App组件
const navigate = useNavigate()
    const godetail = () => {
        navigate('/detail',{
            state: {username: 'qwq'}
        })
    }
    return (
	    <button onClick={godetail}>godetail</button>
	    <Outlet/>
    )
```
2.编程式导航，使用useNavigate方法，为函数指定路径和参数

注，所有路由都通过Outlet标签展现，同router-view
## 传参
### 指定参数配置
router/index.jsx
``<Route path='list/:id' element={<List/>}></Route>``
list/后所有字符串都会解析成id的值（遇到?除外，会单独解出来）
### 查看参数: useLocation
传过来的是一个对象
```
import { useLocation } from 'react-router-dom'
let location = useLocation()
  console.log(location.state)
```
在useLocation方法返回值有hash，pathname，search，state
其中state仅编程式导航中传参才有值，否则为null
pathname:'list/qwqq'
search: "?id=1234"
### 查看指定参数: useSearchParams
指定参数名查询
```
import { useSearchParams } from 'react-router-dom'.
const [searchParams] = useSearchParams()
console.log(searchParams.getAll('id'))
```
useSearchParams方法返回一堆乱七八糟的东西，最好直接用
