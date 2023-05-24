---
title: express
---
挺常用的，在http和nodejs中都有应用
# 安装
``npm i express``
也可以考虑使用nodemon``npm i nodemon``，之后的node指令都用nodemon代替
# 初步
```javascript
const express = require("express")
const app = express();
app.listen(3000,()=>{
    console.log("3000端口监听中")
})
```
## 路由中间件
app.use与app.get方法