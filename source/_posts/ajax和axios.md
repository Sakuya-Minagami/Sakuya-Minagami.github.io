---
title: ajax和axios
---
axios是ajax的一个封装。保证知识点充实，那就都学一下吧
原生ajax
jquary封装ajax
fetch
axios
# 预备知识：http协议
# express
## 用express搭建服务端
### 安装
```
npm init --yes

npm i express
```
### 配置
```
const express = require ('express');
const app = express();

app.get('/server',(request,response)=>{

    // 设置响应头，允许跨域

    response.setHeader('Access-Control-Allow-Origin','*');

    // 设置响应体

    response.send("hello");

})

app.listen(8000,()=>{

    console.log("服务已启动，8000端口监听中")

})
```
### 启动
在根目录下执行
```
nodee express.js
//nodemon express.js
```
# ajax
```
const xhr = new XMLHttpRequest()
            // 初始化 设置请求方法和url
            xhr.open('GET','http://127.0.0.1:8000/server?a=100')
             // 设置请求头
            // content-type设置请求体内容类型，后面为参数查询字符串写法，固定
            xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded')
            // 发送
            xhr.send('a=123')
            // 事件绑定 处理服务端返回结果
            xhr.onreadystatechange = function(){
                if(xhr.readyState === 4){
                    if(xhr.status >=200 && xhr.status <300){
                        // 处理得到的结果 行 头 空行 体
                        console.log(xhr.status)
                        console.log(xhr.statusText)
console.log(xhr.getAllResponseHeaders())
                        console.log(xhr.response)
                        result.innerHTML = xhr.response
                    }
                }
            }
```
# jquary
四个参数分别为，url，参数（对象格式），回调函数，响应体类型
```
$("button").eq(0).click(function(){
	$.get('http://217.0.0.1:000/juqary',{a:100,b:200},function(data){
	console.log(data)
	},'json')
})
```
# axios
[axios中文文档](http://www.axios-js.com/zh-cn/docs/)
