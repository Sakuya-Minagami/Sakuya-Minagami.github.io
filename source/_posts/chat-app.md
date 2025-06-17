---
title： chat-app
---
# 犯的错误
## 端口号与启动问题
react项目用的3000端口，node用的5000端口，之前一直搞混，不知道为什么请求出错，结果是其实没启动node
## 关于bcrypt
是一个用于给password加密的库，问题是有地方打成brcypt，折腾半天报错也报歪了
```
Error: Cannot find module 'D:\qwq\chat-app\server\node_modules\brcypt\index.js'. Please verify that the package.json has a valid "main" entry
```
# react-toastify
一个有意思的react组件库，主要是那个滑动消息比较有意思
```jsx
  import React from 'react';
  import { ToastContainer, toast } from 'react-toastify';

  import 'react-toastify/dist/ReactToastify.css';

  function App(){
    const notify = () => toast("Wow so easy !");

    return (
      <div>
        <button onClick={notify}>Notify !</button>
        <ToastContainer />
      </div>
    );
  }
```
**切记要带上ToastContainer组件**
# node配置
server/index.js
```js
const express = require('express');
const userRoutes = require("./routes/userRoutes");

const app = express();
app.use(express.json());
// 注册路由模块
app.use("/api/auth",userRoutes);

const server = app.listen(process.env.PORT,() => {
    console.log(`Server Start on Port ${process.env.PORT}`)
});

```
## Routes配置
server/routes/userRoutes.js
```js
const { register } = require("../controllers/userController");
// 创建路由对象
const router = require("express").Router();
// 挂载具体路由
router.post("/register",register);

module.exports = router;
```
server/controllers/userController.js
```js
module.exports.register = (req,res,next) => {
    console.log(req.body);
}
```
# 表单提交
public/src/pages/Register.jsx
```js
import axios from "axios";
import { registerRoute } from "../utils/APIRoute";
// ...
if(handleValidation()){
      const { password, confirmPassword, username, email } = values;
      const { data } = await axios.post(registerRoute, {
        username,
        email,
        password,
      });
    };
```
public/src/utils/APIRoute.js
```js
const host = "http://localhost:5000";
export const registerRoute = `${host}/api/auth/register`;
```
# MongoDB连接
server/index.js
```js
mongoose.connect(process.env.MONGO_URL, {
    useNewUrlParser: true,
    useUnifiedTopology: true,
}).then(() => {
    console.log("DB Connection Successfull")
}).catch((err) => {
    console.log(err.message)
})
```
注意MongoDB没开时候会控制台报
``connect ECONNREFUSED ::1:27017``
然后就是去启动MongoDB
连接navcat
# 数据库储存
server/conrtollers/userController.js
检验username和email合法性后，储存email，username和处理后的password
```js
const User = require("../model/userModel");
const bcrypt = require("bcrypt");

module.exports.register = async (req, res, next) => {
    // console.log(req.body);
    try {
        const { username, email, password } = req.body;
        const usernameCheck = await User.findOne({ username });
        if (usernameCheck) {
            return res.json({ msg: "Username already userd", status: false });
        }
        const emailCheck = await User.findOne({ email });
        if (emailCheck) {
            return res.json({ msg: "Email already userd", status: false });
        }
        const hashedPassword = await bcrypt.hash(password, 10);
        const user = await User.create({
            email,
            username,
            password: hashedPassword,
        });
        delete user.password;
        return res.json({ status: true, user });
    } catch (ex) {
        next(ex);
    }
}
```
## model配置
server/model/userModel.js
配置用户字段
```js
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
    username: {
        type: String,
        required: true,
        min: 3,
        max: 20,
        unique: true,
    },
    email: {
        type: String,
        required: true,
        unique: true,
        max: 50,
    },
    password: {
        type: String,
        required: true,
        min: 8,
    },
    isAvatarImageSet : {
        type: Boolean,
        default: false,
    },
    avatarImage: {
        type: String,
        default: "",
    },
})

module.exports = mongoose.model("Users",userSchema)
```
# 头像接口
一直找不到办法，先留着，用四个接口代替吧
## buffer缓冲区
这个是node环境用来传输图片的

## 连续访问同一个图片接口
查看网络发现第一个图片接口照常返回新接口，而第二个图片接口则304，重定向到跟上面一样的接口，也就是说多次调用只能访问同一张
一个思路是提前调用接口并保存其指向url，等到渲染再依次补上