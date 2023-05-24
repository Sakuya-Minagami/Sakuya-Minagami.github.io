---
title: axios与api封装
---
# 配置
在src/request下，在index.js配置根路径
```JavaScript
import axios from 'axios'

let service = axios.create({
    baseURL:"http://localhost:3000/",
    timeout:3000
})
export default service
```
在src/request/api下，home.js或item.js配置axios请求
[axios 的api参考](http://axios-js.com/docs/)
```JavaScript
import service from '..'
// 无参数请求 url使用双引号即可
export function getBanner(){
    return service({
        // 传一个对象
        method:"GET",
        url:"/banner?type=2",
    })
}
// 含参数请求 ：es6模板语法，url使用反引号 参数使用${}引用
// 当然也可以直接用字符串拼接
export function getItemMusicList(data){
    return service ({
        methods:"GET",
        url:`/playlist/track/all?id=${data}&limit=10&offset=1`
    })
}
```
# 引用
```JavaScript
import { getMusicList } from "@/request/api/home.js"
...
methods:{
	async getmusiclist() {
                let res = await getMusicList()
                this.musicList = res.data.result
            }
},
mounted(){
	this.getmusiclist()
}
```
一般用于加载初始的异步数据，也可以在mounted里直接写axios请求，但可能获取太晚报错
不需要初始渲染时，直接写在methods里即可