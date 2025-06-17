---
title: vue-router
---
以下以vue2对应的vue-router3为例
[关于服务器配置与路由模式](https://v3.router.vuejs.org/zh/guide/essentials/history-mode.html)
# 构建
在router/index.js中
```js
const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>' }

const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
]

const router = new VueRouter({
  routes 
})

const app = new Vue({
  router
}).$mount('#app')
```
可见component直接对应路由对应的组件，准确说是它的html部分
也可以使用import引入
``component: () => import('../views/AboutView.vue')``

可以使用``this.$router``访问路由器（使用router实例等效），
使用``this.$route``访问当前路由
对于后者，
```js
this.$route.fullPath: "/itemMusic/2670054457"//访问路径
this.$route.name: "ItemMusic"//访问组件名
this.$route.params.id: "2670054457"//访问参数
this.$route.path: "/itemMusic/2670054457"
```
# 路由结构
## 命名路由
```js
path: '/user/:userId',
      name: 'user',
      component: User
```
可使用
``router.push({ name: 'user', params: { userId: 123 } })``
## 嵌套路由
```js
 path: '/user/:id',
      component: User,
      children: [
        {
          // 当 /user/:id/profile 匹配成功，
          // UserProfile 会被渲染在 User 的 <router-view> 中
          path: 'profile',
          component: UserProfile
        },
```
路由可以嵌套路由，同样router-view也可以嵌套
子路由路径默认父路由根路径
### 重定向与别名
```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
  // 甚至可以使用回调函数动态跳转路由
  { path: '/a', redirect: to => {
      // 方法接收 目标路由 作为参数
      // return 重定向的 字符串路径/路径对象
    }}
})
```
一般来说，使用通配符* 作为路径来重定向，直接定向到常用页面

也有一种办法，使路径/a对应/b的组件，虽然这么折腾可能更麻烦
```js
routes: [
    { path: '/a', component: A, alias: '/b' }
]
```
  
## 命名视图：组件化视图
```js
path: '/',
components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
...
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
```
## 动态路由：将url解析为参数
```js
const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id' }
  ]
})
```
user/后的会被解析为参数，但也可以跨url传参
```js
/user/:username/post/:post_id
/user/evan/post/123
{ username: 'evan', post_id: '123' }
```
注，跳转路由时传参，组件会复用，也就是不会执行钩子函数
可以使用watch检测代替
# 路由使用
声明式导航：``<router-link :to="...">``
编程式导航：``router.push(...)``
## 路由与历史记录
通常路由跳转会留下历史记录，但``router.replace（location）``不会
路由可以通过历史记录直接跳转``router.go(n)``
## 路由守卫
### 全局路由守卫：router.beforeEach
```js
const router = new VueRouter({ ... })
router.beforeEach((to, from, next) => {
})
```
可通过在beforeEach中写路由守卫的逻辑
to代表目标路由
from代表源路由
next()方法根据传入参数不同，执行不同效果
``next()``无视守卫跳转
``next('/')``跳转指定路由
``next(false)``中断路由跳转，跳回from对应路由
### 路由独享守卫：router.beforeEnter
```js
routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
      }
    }
  ]
```
直接在路由中定义即可
## 路由元信息
常配合路由守卫使用，配置在路由中
```js
path: 'bar',
omponent: Bar,
meta: { requiresAuth: true }
...
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    if (!auth.loggedIn()) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    next()
  }
})
```
