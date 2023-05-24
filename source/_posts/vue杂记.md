---
title: vue杂记
---
要经常捡东西回来哦
# 父子通信
[十种办法](https://blog.csdn.net/qq_37288477/article/details/86630428)
## props 父传子参数
在父组件的子标签中写要传的参数，空格隔开
```
<home title="" date=""></home>
```
子组件在props接收
```
<script>
	export default {
	  name: 'Home',
	  props: ['title','date']
	}
</script>
```
## $emit 子用父函数
在子组件中$(name,param),name为父组件函数名，param为参数，可以在父组件中用 $event访问这个值
# $nextTick
用于渲染以后，在后续视图更新执行的回调函数