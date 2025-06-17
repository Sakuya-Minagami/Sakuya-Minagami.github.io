---
title: diff算法
---
来自一个视频的笔记
[b站：diff算法源码实现](https://www.bilibili.com/video/BV1dV411a7mT)

# dom树结构

```js
const VDom = createElement('ul',{
	class: 'list',
	style: 'width: 300px;'
},[
	createElement('li',{
		class: 'item',
		'data=index': 0
	},[
		createElement('p',{
		class: 'text'
		},['第一个列表项'])
	]),
	createElement('li',{
		class: 'item',
		'data-index': 1
	})
])
const realDom = render(VDom)
```
利用createElement()统一创建dom节点，第一个参数为标签名，第二个参数为一个对象，包含类名、style等属性，第三个参数为子节点数组。
# render()解析
```js
function render(VDom){
	const { type, props, children } = VDom;
	const el = document.createElement(type);
    // 处理属性
	for(let key in props){
		setAttrs(el,key,props[key]);
	}
    // 处理子节点
    children.map((c) => {
        c = c instanceof Element
            ?
            render(c)
            :
            document.createTextNode(c);// 文本节点单独考虑
        el.appendChild(c);
    })
    return el
}
```
## setAttrs()
为了区分value情况专门写一个函数
```js
function setAttrs(node, prop, value){
	switch(prop){
		case 'value':
			if(node.tagName === 'INPUT' ||node.tarName === 'TEXTAREA'){
				node.value = value;
            } else {
                node.setAttribute(prop,value);
            }
            break;
        case 'style': 
            node.style.cssText = value;
            break;
        default:
            node.setAttribute(prop,value);
	}
}
```
# createElement()
```js
function createElement(type, props, children){
    return new Element(type,props,children)
}
class Element {
	constructor (type,props,children){
		this.type = type;
		this.props = props;
		this.children = children;
	}
}
```
