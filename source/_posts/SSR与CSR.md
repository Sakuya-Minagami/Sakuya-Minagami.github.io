---
title: SSR与CSR
---
补点知识，或者说当做笔记

服务端渲染已经是几年前的内容了，只不过近期又火起来了
*当我转前端的时候已经是前后端分离的时代了*
现在请求数据都是在前后端接口上下功夫，但是以前前后端不分离，都是直接在页面脚本请求和读写文件

来个那个时代的php代码做例子
```php
<table>
	<thead>
		<th>id</th>
		<th>name</th>
	</thead>
	<tbody>
	<?php
		$txt = file_getcontents("1.json");
		$arr = json_decode($txt,true);
		$res = "";
		foreach($arr as $item){
			$res = $res."<tr><td>".$item['id']."</td><td>".$item['name']."</td><tr>";
		}
		echo $res;
	?>
	</tbody>
</table>
```
服务端读到php标签时，先执行脚本，再用echo返回的内容替代整个tbody
它的特点是在服务端就填充dom，执行好脚本再发送给浏览器，相当于只是一个静态页面
我们在浏览器看源码看到的都是已经处理好写死的数据
*这也就和ajax，动态页面响应的兴起关联了*


# 一点问答
**概括地说，SSR内容主体还是CSR，只是多了对服务端的考量，了解双端差异即可**
## 为什么要有同构？
指的是客户端和服务端执行一套js，否则客户端会拿到空的函数
## 脱水注水指的是什么？
（摘抄）
Next.js 在做服务器端渲染的时候，页面对应的 React 组件的 getInitialProps 函数被调用，异步结果就是“脱水”数据的重要部分，除了传给页面 React 组件完成渲染，还放在内嵌 script 的__NEXT_DATA__ 中，这样，在浏览器端渲染的时候，是不会去调用 getInitialProps 的，直接通过__NEXT_DATA__ 中的“脱水”数据来启动页面 React 组件的渲染。

这样一来，如果 getInitialProps 中有调用 API 的异步操作，只在服务器端做一次，浏览器端就不用做了。
## next.js 中getinitialProps作用
[知乎](https://www.zhihu.com/question/54877807)
[CSDN](https://blog.csdn.net/Winne_Shen/article/details/88424261)
