---
title: vuex
---
vuex其实跟redux差不多，目的也很简单，用来方便管理数据流，好调试bug
当然，在小项目中自然不需要这么麻烦，原生vue就够了
以下以vue2对应的vuex3为标准
[api说明](https://vuex.vuejs.org/zh/api/)
# 安装
``npm install vuex --save``
# 概念与使用
## store
在使用之前，先谈一下store的底层实现原理
最初始的想法是，在computed中初始化变量，变量变化引起组件更新就能实现实时访问和监控变量
```
computed: {
    count () {
      return store.state.count//注意此处使用store
    }
  }
```
文档对这种办法提出了不合理之处
``然而，这种模式导致组件依赖全局状态单例。在模块化的构建系统中，在每个需要使用 state 的组件中需要频繁地导入，并且在测试组件时需要模拟状态。``
在新的方案中，vue允许将对象store注册给"store"属性，再将它注入到每个子组件中，也就是说每个子组件实例都有了store属性
```
computed: {
    count () {
      return this.$store.state.count//此时使用了$store
      //也因为this指向的就是vue实例，所以也可以不写this
    }
  }
```
方案在main.js中实现
```
const app = new Vue({
  router,
  store,//es6,<=>store:store
  render: h => h(App)
}).$mount('#app')
```
## state
代表一个全局变量的库，可以初始化，可以访问，可以修改
只要在store初始化，在子组件无需注册就可以访问
一般有两种办法：
直接使用全局变量``$store.state.value``
或者在computed声明，在组件中使用重新命名的变量
```
computed: {
    value () {
      return this.$store.state.value
    }
  }
```

注，state也可使用回调函数，其返回值作为根state，根的概念在modules模块化会出现
### mapState:更常用的方案
```
import { mapState } from 'vuex'
...
computed: {
            ...mapState(['playList','playListIndex')
        }

```
声明以后，就可以在组件中直接使用变量名
顺便，对象展开运算符[阮一峰博客](https://es6.ruanyifeng.com/#docs/array)
### vue3中的写法
值得一提的是，vue2所有方法都保留，vue3只是又封装了一堆东西，怪麻烦的[封装的storeState原理](https://blog.csdn.net/m0_65382120/article/details/125820878)
```
import { useStore } from 'vuex'

export default {
  setup () {
    const store = useStore()//此时可以直接使用store对象了
  }
}
```
## getters（不太重要）
相当于state的计算属性，依赖于state，同样可以缓存
```
//偷懒就抄文档了，实际上直接找store位置初始化就好了
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```
像state一样使用就可以了``store.getters.value``
### 样例
特别地，还能用来做数组筛选
```
getters: {
  // ...
  getTodoById: (state) => (id) => {
    return state.todos.find(todo => todo.id === id)
  }
}
```
此时getters返回函数getTodoById()，可以调用也可以传参了
### mapGetters
对于mapGetters()，用法同state，传要用的属性名的数组进去就可以了``...mapGetters([ 'doneTodosCount', 'anotherGetter',])``
顺便还可以换个新名字``...mapGetters({doneCount: 'doneTodosCount'})``不过注意此时用的是大括号

注，getters用上面的函数式方法时，访问即调用，不缓存
## mutations
mutation是修改state的唯一方案，定义修改state的回调函数
```
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```
调用这些函数需要用``store.commit('increment')``

注，mutation只允许同步函数，不允许异步
### payload载荷
在commit函数中，第一个传函数名，后面均为传参``store.commit('increment', 10)``
但是这样要传一堆，不如直接传个对象，就有了新的写法
```
store.commit({
  type: 'increment',//type即定义的回调函数名
  amount: 10
})
```
注意此时第一个传参是一个对象
#### 分发
允许组件调用函数修改state的过程称为分发，说成授权会更好理解一些
在payload中，第一种数组形式称为载荷方式分发，第二种对象形式称为对象形式分发
使用mapxxx的方式，没命名，就叫组件式分发好了
### mapMutations
``import { mapMutations } from 'vuex'``
在methods中声明即可`` ...mapMutations``
## actions
actions类似mutations，但是它commit的是mutations
相当于又封装了一层，这一层就可以为异步操作争取空间了
```
actions: {
    increment (context) {
      context.commit('increment')
    }
  }
```
传入参数context是一个store实例，同样也可以访问其中的state，mutations，以及commit方法
### 调用
要调用actions，需要使用dispatch方法``store.dispatch('increment')``
这代表它会再套一层commit方法
actions的特点是允许异步操作，包括再套一层需要严格异步的commit
```
actions: {
  incrementAsync ({ commit }) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }
}
```
### 分发
actions分发同mutations
## modules
模块化的意义在于管理大量的变量和方法，在大工程里按照功能和需求把它们分类并封装出来管理
```
const moduleA = {}
const moduleB = {}
const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})
```
用``store.state.a``形式访问即可，相当于套了一层数组
### 构造方法
完整的store构造应该长这样，这意味着modules可以层层嵌套
```
{
  key: {
    state,
    namespaced?,
    mutations?,
    actions?,
    getters?,
    modules?
  },
  ...
}
```
modules变成树以后，自然就会有访问根节点的问题了
### 模块局部状态
modulesA中
```
mutations: {
    increment (state) {
      state.count++
    }
  },
```
此时的state是一个形参，文档里叫模块的局部状态对象

可以通过rootState访问根节点modulesA
```
actions: {
    incrementIfOddOnRootSum ({ state, commit, rootState }) {
      if ((state.count + rootState.count) % 2 === 1) {
        commit('increment')
      }
    }
  }
```
[这是一篇跨模块调用示例](https://blog.csdn.net/Wbiokr/article/details/80685)
### 命名空间
其实在上面的例子中可以看到，模块化便于管理但也让结构更为复杂
实际上，例如commit方法，每个modules实例都有，传参是否有限制呢？答案是否定的，因为所有定义的state和mutation都是注册在**全局命名空间**的
这意味着命名不能引起冲突，但也有新的方案使名字注册到指定路径上
```
methods: {
	...mapMutations({
		'name': 'users/name'
	})
}
```
其实也可以在参数中直接使用路径
```
const actions={
  editName({commit,rootState},payload){
    commit('About/changeComponey','newComponey',{root:true}) 
    console.log(rootState.About)
  }
}
```
另外，这种办法也可以把store拆开，拆分成多个js文件导出
`state.js`
`mutation.js`
# 源码解析
未完待续