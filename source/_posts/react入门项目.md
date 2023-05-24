---
title: react入门项目
---
# 配置环境
```bash
PS D:\review\react\logo-management> npm init vite
Need to install the following packages:
  create-vite@3.2.1
Ok to proceed? (y) y
√ Project name: ... logo-management
√ Select a framework: » React
√ Select a variant: » TypeScript

Scaffolding project in D:\review\react\logo-management\logo-management...

Done. Now run:

  cd logo-management
  npm install
  npm run dev
PS D:\review\react\logo-management> npm i

added 86 packages in 14s
PS D:\review\react\logo-management> npm i reset-css 

added 1 package in 2s
S D:\review\react\logo-management> npm i --save-dev sass  

added 16 packages in 2s

```
## 路径别名
```bash
PS D:\review\react\logo-management> npm i -D @types/node

added 1 package in 1s
```
vite.config.ts下
```JavaScript
import * as path from 'path'

export default defineConfig({
  plugins: [react()],
  resolve:{
    alias:{
      "@":path.resolve(__dirname,'./src')
    }
  }
})

```
路径提示：
ts.config.json下
```
"baseUrl": "./",
    "paths": {
      "@/*":[
        "src/*"
      ]
    }
```
## scss模块化引入
在文件夹Comp1下，两个文件comp1.modules.scss和index.tsx
index.tsx下
```JavaScript
// import "./comp1.scss"//{} 全局引入

// 模块引入
import style from "./comp1.modules.scss"
function Comp(){
    return (
        <div className="{style.box}">
            <p>这是Comp1的内容</p>
        </div>
    )
}

export default Comp
```
相当于scoped，独立使用样式
*其实就是引入的style不同而已*
## AndDesign
```bash
PS D:\review\react\logo-management> npm install antd --save

added 65 packages in 12s
PS D:\review\react\logo-management> npm install --save @ant-design/icons

up to date in 1s
```
只需要引入组件名，不需要引入样式了
```
import { Button } from 'antd'
import { UpCircleOutlined } from "@ant-design/icons"
// import 'antd/dist/reset.css'
```
*这个图标库挺卡的*
### 按需引入
```bash
PS D:\review\react\logo-management> npm install vite-plugin-style-import@1.4.1 -D

added 25 packages in 3s
PS D:\review\react\logo-management> npm i less@2.7.1 -D

added 10 packages in 3s
```
vite.config.ts下
```typescript
import styleImport,{AntdResolve} from 'vite-plugin-style-import'

export default defineConfig({
  plugins: [
    react(),
    styleImport({
      resolves: [
        AntdResolve()
      ],
  }),
  ],
  resolve:{
    alias:{
      "@":path.resolve(__dirname,'./src')
    }
  }
})
```
# 路由配置
```bash
PS D:\review\react\logo-management> npm install react-router-dom@6

added 1 package, removed 10 packages, and changed 2 packages in 5s
```
路由视图：创建views文件夹，包含Home.tsx和About.tsx
```
const Home = () => {
    return (
        <div className="home">
            这是home组件
        </div>
    )
}

export default Home
```
配置路由结构：根目录下创建router文件夹，包含index.tsx
注意点有视图引入与路由结构
```typescript
import App from "../App"
import Home from "../views/Home"
import About from "../views/About"
import { BrowserRouter,Routes,Route } from "react-router-dom"

const baseRouter = () => (
    <BrowserRouter>
        <Routes>
            <Route path="/" element={<App/>}>
                <Route path="/home" element={<Home/>}></Route>
                <Route path="/about" element={<About/>}></Route>
            </Route>
        </Routes>
    </BrowserRouter>
)

export default baseRouter
```
路由引入：main.tsx下
```typescript
ReactDOM.createRoot(document.getElementById('root') as HTMLElement).render(
  <React.StrictMode>
    <Router />
  </React.StrictMode>
)
// 原本中间是<App />
```
## 路由重定向
router/index.tsx下
```typescript
import { BrowserRouter,Routes,Route,Navigate } from "react-router-dom"
...
<BrowserRouter>
        <Routes>
            <Route path="/" element={<App/>}>
                <Route path="/" element={<Navigate to="/home" />}></Route>
                <Route path="/home" element={<Home/>}></Route>
                <Route path="/about" element={<About/>}></Route>
            </Route>
        </Routes>
    </BrowserRouter>
```
如果不使用navigate的重定向，在localhost3000并不会显示Outlet组件
注意navigate是一个组件名，还要配置路径