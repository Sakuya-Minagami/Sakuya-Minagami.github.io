---
title: react复习
---
其实react东西也挺杂的，忘了不少，复习加总结吧
# 受控与非受控组件
## 受控组件
先放文档原话

在 HTML 中，表单元素（如`<input>`、 `<textarea>` 和 `<select>`）通常自己维护 state，并根据用户输入进行更新。而在 React 中，可变状态（mutable state）通常保存在组件的 state 属性中，并且只能通过使用``setState``来更新。  
我们可以把两者结合起来，使 React 的 state 成为“唯一数据源”。渲染表单的 React 组件还控制着用户输入过程中表单发生的操作。被 React 以这种方式控制取值的表单输入元素就叫做“受控组件”。

总结一下，受控的问题就是为了解决数据更新与视图更新的问题
**受控问题与state密不可分**
在学习setState时，我们知道光修改表单是不能修改数据的，react的状态只能通过setState修改，这和vue的数据双向绑定不同
受控组件的流程是
1，可以通过在初始state中设置表单的默认值
2，每当表单的值发生变化时，调用onChange事件处理器，
3，事件处理器通过事件对象e拿到改变后的状态，改变state；
4，setState触发视图更新，完成表单组件值的更新
*注意到，所有的操作都绕开了操作dom，只修改react组件*

来个例子
```js
 export default class InputItem extends React.Component{
  constructor(props){
    super(props);
    this.state = {
      value: ""
    }
  }
  componentWillReceiveProps(nextProps){
    this.setState({
      value: nextProps.value
    })
  }
  _onChange(evt){
    this.setState({
      value: evt.target.value
    })
  }
  render(){
    return (
      <input type="text"
        value={this.state.value}
        onChange={this._onChange.bind(this)}/>
    );
  }
}
```
### 父子组件形式
```ts
//父组件
class Parent extends Component {
    constructor(props) {
        super(props);
        this.state = {value: 1};
        this.onChangeHandle = this.onChange.bind(this);
    }
    onChange(event) {
        //处理值
        if (改) {
        this.setState({value: event.target.value});
        }
    }
    render() {
        return
            <NumberInput value={this.state.value} onChange={this.onChangeHandle} />
        ;
    }
}
 
//子组件NumberInput
class NumberInput extends Component {
    constructor(props) {
        super(props);
        this.state = {
        value: props.value
        };
        this.onChangeHandle = this.onChange.bind(this);
    }
    //接收到新的属性时更新state
    componentWillReceiveProps(nextProps) {
        this.setState({
        value: nextProps.value
        });
    }
    onChange(event) {
        //通知父组件值更新了
        this.props.onChange(event);
    }
    render() {
        return
            <input type="text" className="NumberInput"
            value={this.state.value}
            onChange={this.onChangeHandle} />
    }
}
```
## 非受控组件
原话
在大多数情况下，我们推荐使用 **[受控组件](https://link.zhihu.com/?target=https%3A//zh-hans.reactjs.org/docs/forms.html%23controlled-components)** 来处理表单数据。在一个受控组件中，表单数据是由 React 组件来管理的。另一种替代方案是使用非受控组件，这时表单数据将交由 DOM 节点来处理。

特点：非受控组件通常和ref结合在一起，通过node.value方式来获取dom节点的值，这种方式适合不需要知道实时状态，只需要提交的表单组件。
```js
import React, { Component } from 'react';
 
class UnControlled extends Component {
    handleSubmit = (e) => {
        console.log(e);
        e.preventDefault();
        console.log(this.name.value);
    }
    render() {
        return (
            <form onSubmit={this.handleSubmit}>
                <input type="text" ref={i => this.name = i} defaultValue="BeiJing" />
                <button type="submit">Submit</button>
            </form>
        );
    }
 
}
 
export default UnControlled;
```
### 父子组件
```ts
//父组件
class Parent extends Component {
    render() {
        return
            <NumberInput defaultValue=20 />;
    }
}
 
//子组件NumberInput
class NumberInput extends Component {
    render() {
        return
            <input type="text" className="NumberInput"
            defaultValue={this.props.defaultValue} />
    }
}
```
**所以个人理解是，
受控组件关注表单过程，非受控组件关注表单结果
受控组件从react实例入手，非受控组件从dom元素属性入手**

**注意了，类组件才有状态，才有受控的可能**


# 钩子函数
## useState
这个稍微记一下用法，至于实现会是个考点，可能难点
```ts
import { useState } from 'react'
function App() {
    const [count, setCount] = useState(0)
    const handleClick = () => {
        setCount(count + 1)
        // 传入一个函数，更新的值是基于之前的值来执行
        // setCount(count => count + 1)
    }
    return (
    	<div>
        	<h4>count: {count}</h4>
            <button onClick={ handleClick }>点击更新状态</button>
        </div>
    )
}
```
**set操作是异步更新的**
*详情见react：state*
# useEffect
用法就不写了，写点注意的吧
首先useEffect只能叫钩子函数，叫生命周期不准确

# useCallback
核心问题在于：
**什么函数使用？什么时候使用？**
讲得非常好[CSDN：useCallback深度解读](https://blog.csdn.net/lixiaonaaa/article/details/126537858)

# useRef
[useRef与useState区别与联系](https://baijiahao.baidu.com/s?id=1758989293825938832)
[b站：useRef](https://www.bilibili.com/read/cv23863547)
[react闭包陷阱](https://blog.csdn.net/weixin_45696837/article/details/128243176)
**useState 和 useRef 是 React Hooks 的两种最常见的使用方式，用于管理状态和操作 DOM 元素。**
**当更新 current 值时并不会 重新渲染视图 ，这是与 useState 不同的地方。**
相当于类组件中react.createRef()
含义是保存引用值，作用是提供一个可以在函数式组件中访问的全局变量，而不必渲染组件
## 访问dom或组件
绑在dom上，可以用ref.current访问
```tsx
import React, { useState, useEffect, useMemo, useRef } from 'react';

export default function App(props){
  const [count, setCount] = useState(0);

  const doubleCount = useMemo(() => {
    return 2 * count;
  }, [count]);

  const couterRef = useRef();

  useEffect(() => {
    document.title = `The value is ${count}`;
    console.log(couterRef.current);
  }, [count]);
  
  return (
    <>
      <button ref={couterRef} onClick={() =>
	       {setCount(count + 1)}}>Count: {count}, 
	       double: {doubleCount}
       </button>
    </>
  );
}
```
可以打印在控制台看到ref.current返回一个dom
## 组件通信
使父组件访问子组件方法
```jsx
const Children = forwardRef((props, ref) => {
	return <input {...props} ref={ref} />
})

function MyApp () {
  const childrenRef = useRef(null);

  function childrenClick() {
  	childrenRef.current.focus();
  }

  return <Children ref={childrenRef} />
} 
```
不过我感觉跟传统父子通信也没区别？
