---
title: webpack
---
没想到内容很广，也很陌生
[webpack入门](https://cloud.tencent.com/developer/article/1195063)
[知乎：了解webpack](https://zhuanlan.zhihu.com/p/269434612)
[webpack教程资源收集](https://segmentfault.com/a/1190000011530762)
# 用途
webpack是一个前端构建工具，前端构建工具就是把开发环境的代码转化成运行环境代码。
处理情况：
1.代码压缩
将JS、CSS代码混淆压缩，让代码体积更小，加载更快
2.编译语法
编写CSS时使用Less、Sass，编写JS时使用ES6、TypeScript等，这些标准目前都无法被浏览器兼容，因此需要构建工具编译，例如使用Babel编译ES6语法。
3.处理模块化：
CSS和JS的模块化语法，目前都无法被浏览器兼容。因此开发环境可以使用既定的模块化语法，但是需要构建工具将模块化语法编译为浏览器可识别形式。例如使用webpack、Rollup等处理JS模块化。
# 概念
1 Entry
入口(Entry)指示Webpack以哪个文件作为入口起点分析构建内部依赖图并进行打包。
2 Output
输出(Output)指示Webpack打包后的资源bundles输出到哪里去，以及如何命名。
3 Loader
Loader让Webpack能够去处理那些非JavaScript语言的文件，Webpack本身只能理解JavaScript。
4 Plugins
插件(Plugins)可以用于执行范围更广的任务，插件的范围包括从打包和压缩，一直到重新定义环境中的变量等。
5 Mode
模式(Mode)指示Webpack使用相应模式的配置。分为development和production两种模式，下面分别进行简述。

development: 开发模式，能让代码本地运行的环境，会将process.env.NODE_ENV的值设为development，同时启用NamedChunksPlugin和NamedModulesPlugin插件；

production: 生产模式，能让代码优化运行的环境，会将process.env.NODE_ENV的值设为production，同时启用FlagDependencyUsagePlugin、FlagIncludedChunksPlugin、ModuleConcatenationPlugin、NoEmitreplaceStringsPlugin、OccurrenceOrderPlugin、SideEffectsFlagPlugin和UglifyJsPlugin插件。