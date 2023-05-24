---
title: remix
---
[remix样例](https://juejin.cn/post/7042884869713559560#heading-12)
[在remix配置tailwindcss](https://www.netlify.com/blog/how-to-use-tailwind-css-with-remix/)
# 路由系统
定义路由有两种办法：remix.config.js和文件夹创建路由
## config.js
```
module.export = {
	async routes(defineRoutes) { 
	return defineRoutes((route) => { 
	//参数一为路径，参数二为文件，参数三为嵌套路由
	route("test/:path", "routes/test/index.tsx", () => { 
    route("relative/child1", "routes/test/child.tsx");
	route("relative/child2", "routes/test/child2.tsx"); }); }); },
```
