---
title: http协议
---
比较重要且常用，拿出来专门留一篇
[理解一个简单的网页请求过程](https://www.cnblogs.com/orchid/archive/2012/04/21/2461442.html)
[Chrome浏览器查看网络请求](https://xie.infoq.cn/article/97bf7543d1d9621572852ca39)

# 尚硅谷课程
## 请求报文
重点：格式与参数
请求报文包括
### 请求行
1.请求类型：
GET ，POST等
2.url路径
即?后面传的一堆参数
3.http版本
1.1或1.0
### 请求头
比如：
HOST: bilibili.com
Cookie: name=qwq
Content-type: application/x-www-form-urlencoded
User-Agent: chrome 83
### 空行，必须存在
### 请求体
GET请求中请求体为空
POST请求可以不为空
username=admin&password=admin
## 响应报文
### 响应行
1.http版本
HTTP/1.1
2.响应状态码
200
3..响应状态字符串
OK
### 响应头
Content-Type: text/html;charset=utf-8
Content-length: 2048
Content-encoding: gzip
### 空行
### 响应体
传一个html
```
<html>
...
</html>
```