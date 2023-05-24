---
title:  部署vue项目到GitHub
---

### 初次配置
## 项目配置
记得模式改为hash模式
vue.config.js中(否则项目可能空白)
```
publicPath:'.',
```
package.json中，“scripts”内
```
"predeploy": "npm run build",

"deploy": "gh-pages -d dist -r git@github.com:Sakuya-Minagami/cloudnotes.git -b gh-page"
```
其中url要用git否则可能连接不上，-b表示切换分支，gh-page为新建的分支名

## 发布到GitHub
网络连接超时情况
```
yarn config set registry https://registry.npm.taobao.org
npm install yarn -g
```

安装gh-pages
```
yarn add gh-pages -D
```
部署
```
yarn deploy
```

### 另一种发布方法
在发布的文件夹根目录打开git bash
```
git init
git add .
//空格和.不要漏
git remote add origin git@github.com:Sakuya-Minagami/cloudnotes.git
git clone git push -u origin main
git clone git@github.com:Sakuya-Minagami/cloudnotes.git
```

记得回到项目关掉lint，阻止命名审核
直接eslint
在vue.config.js中
```
lintOnSave: false
```
记得重启项目
eslintrc.js中extends注释掉'@vue/stadard'
rules中
```
'vue/multi-word-component-names': 'off'
```
还有未使用警告
```
"no-unused-vars": “off”
```
回到git bash
```
git commit -m "second"
//second为提交信息
git push origin master
```
## GitHub处理
在仓库界面进入setting，page，选择分支和主题
