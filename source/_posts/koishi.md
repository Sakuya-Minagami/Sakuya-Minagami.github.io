---
title: koishi
---
写一下koishi的开发流程
# 安装
[koishi控制台](https://k.ilharp.cc/win.msi)
管理员模式cmd
```
corepack enable
corepack prepare --all --activate
npm config set registry https://registry.npmmirror.com
```
指定目录下
```
yarn create koishi --mirror https://ghproxy.com/https://github.com
```
一路回车即可
# 文件结构
koishi-app下
external放自定义插件
src放本地控制台源码
# 插件开发
``npm run dev`` 启动控制台
``npm setup plugin`` 创建插件，其中plugin为自定义插件名
external/plugin为插件文件夹，其下src/index.ts为插件源码
## 指令
所有自定义指令在apply下定义
```ts
export function apply(ctx: Context) {
  ctx.command('艾草')
  .action(() => {
    return '爱丽丝想艾草'
  })
}
```
# 一点想法
## 卡池的数据结构
需求：一个角色有唯一的url，但是有若干个昵称，需要查询昵称返回url
我的想法：封装成大对象data，每个角色用name表示，name有唯一url属性和nickname数组，里面有若干字符串
```js
// name封装在data数组里
const name = {
	const url = '';
	const nickname = ['','','']
}
function query(queryname){
	for (name in data) {
		for(el in name.nickname){
			if(el === queryname) return name.url
		}
	}
	return undefined
}
```
大佬的想法：作两个表，一个表键值对为name:url,一个表键值对为nickname:name
通过两个表相互映射实现查找
```js
const IdxMap = {
	单车白子 : 'url_1',
	单车小仓唯 : 'url_1'
}
const UrlMap = {
	url_1 : 'http://1',
	url_2 : 'http://2'
}
```
这种方案可以把添加角色和添加昵称的工作独立开，确实是值得参考的办法
*顺便，大量数据当然适合用数据库了，查找本来就是数据库的强项，小数据可以直接加载*