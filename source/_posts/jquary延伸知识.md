---
title: jquary延伸知识
---
[jquary设计模式上](https://blog.csdn.net/baidulixueqin/article/details/122401805?ops_request_misc=&request_id=&biz_id=102&utm_term=jQuery%20%E4%B8%AD%E7%9A%84%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~sobaiduweb~default-2-122401805.article_score_rank_blog&spm=1018.2226.3001.4450)
[jquary设计模式下](https://blog.csdn.net/baidulixueqin/article/details/122402784)
[jquary设计模式源码分析](https://blog.csdn.net/weixin_44228042/category_10745276.html)
[JQuery编码规范与最佳实践](https://www.w3cschool.cn/kesyi/kesyi-q3sa24s1.html)
# jquary设计模式
1.不用new的构造函数，这个模式没有专门的名字
2.$(支持多种参数)，这个模式叫重载
3.用闭包隐藏细节，这个模式没有专门的名字
4.getter/setter
5.别名
6.适配器
# 构造
```
jQuery = function( selector, context ) {
// The jQuery object is actually just the init constructor 'enhanced'
    return new jQuery.fn.init( selector, context, rootjQuery );
}
```
这是jquary的构造函数，但是真正的构造函数显然是jQuery.fn.init
用这样的办法，就不需要使用new jquary来创建jquary实例了，

# 7个重载方法
1.$() 
返回包含document节点对象
2.$(element)
将一个或多个DOM元素转化为jquary对象，即jquary包装集
	特别地，$(document)可将xml文档和window对象作为有效参数
3.$(cllback)
即jquary(document).ready(callback)简写
	注，ready是DOM树结构加载完成就执行，window.onload是所有元素加载完（包括图片资源等），只执行一次
4.$(expression,[context])
表示
5.$(html)
表示创建一个节点
6.$(html,props)
表示对创建节点增加属性，如
```
$("<input>",(type:"text",name="username"))
```
7.$(html,[ownerDocument])
表示创建DOM元素在指定文档
# 浏览器特性检测
[jQuery 工具--浏览器及特性检测](https://blog.csdn.net/tjj3027/article/details/79280719)
# 链式编程原理
只要在每个方法中，都返回对象本身，即``return this``
那么下一个函数即可再次调用
注：JavaScript默认返回undefined

以下为一个样例，其中api为对象，其中的方法即返回外层对象
```JavaScript
const api = {
  addClass(className) { //主要代码
    ...
    return api 
    }
 }
 return api
  
接口
const api = jQuery('.test')
api.addClass('red').addClass('blue')//链式操作

```