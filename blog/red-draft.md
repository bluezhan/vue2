
## 集Vue+TyleScript+Webpack+...开发的资源流水线  


### Vue+  

Vue是东西呢？http://cn.vuejs.org/   

从Vue2.0开始吧，之前有用过Vue1.0+。  

由尤雨溪开发并维护的前端框架，在这两年2015、2016国内大大被重用和提起。 

[Vue 2.0 中文版](https://vuefe.cn/)

#### 在国内有些大公司已经引进Vue来处理业务：  

UC浏览器、稀土掘金、  
滴滴, 还出了一本书 Vue.js 权威指南   
饿了么，开源了一个基于Vue的UI库 GitHub - ElemeFE/element: Desktop UI elements for Vue.js 2.0   
阿里的 weex GitHub - alibaba/weex: A framework for building Mobile cross-platform UI   
GitLab选择了Vue https://about.gitlab.com/2016/10/20/why-we-chose-vue/   

荔枝FM，移動端部分使用Vue   

B 站的三个项目 _(•̀ω•́ 」∠)_：   
圈子：哔哩哔哩兴趣圈，二次元兴趣交友社区  
直播（仅首页与活动页）：哔哩哔哩直播，二次元弹幕直播平台  
周边商城：bilibili－周边商城   

上面的信息基本来自知乎》https://www.zhihu.com/question/33264609  

#### 更新项目

简书  
饿了吗  
网易云音乐  
知乎日报  

Node爬虫接口: https://ecitlm.github.io/SpliderApi/#/

### 开始Vue

已经在知乎发表了一篇文章 [新手向：Vue 2.0 的建议学习顺序](https://zhuanlan.zhihu.com/p/23134551)    

我在这里就加几点：
- 前端基础就不说了，HTML+CSS+JavaScript   
- 有过开发插件、模块化（module）、组件、控件  
- Node、工程化工具（grunt|gulp|webpack） 
- ....

好啦~~~

一般其实呢  
最重要的就是会排查错误和查找问题，也要会最新资讯。  

npm 和cnpm 的关系   
sass、node-sass 在npm install的时候为什么老是错误   

翻墙和网络好  


哈哈哈，看到下面有人评论：Vue 不难学，难的是配置 Webpack……哈哈。    

所以在学前端的时候，还需要学浏览器、 HTTP、数据、网络 ...

### 更类似的东东

之前很久之前就是JSP，有看过、接触过JSP的同学们都知道   
JSP 的语法就是在HTML里加入自定义的东西和加入了动态语言  

哦，好吧，我们就说MVC模式，还有MVVM模式............
哎呀，可能早期还会接触到backbone框架的人，或者已经不是很重要了   
重要的是接触过angular的人，这一下就会更加很好非常棒地.....  
就是很棒，别说了        

我想说的就是：

- 分离
- HTML动态解析模板
- 结构化
- 模块化
- 工程化

### 一步一步来...

#### Vue 基础

  有个Vue社区 http://www.vue-js.com/  

  2.0中文社区，上面有提到，再来再次给出《[Vue 2.0 中文版](https://vuefe.cn/)》  
  
  看一下2.0的API：https://vuefe.github.io/api/  

#### 组件

#### vue-router

来，链接在这里：[vue-router 2](http://router.vuejs.org/zh-cn/)  


__路由对象和路由匹配__  

路由对象，即$router会被注入每个组件中，可以利用它进行一些信息的获取。如

|属性|说明|
|-----------------------------------|:-------------------------------:|
|$route.path    |当前路由对象的路径，如'/view/a'|
|$rotue.params  |关于动态片段（如/user/:username)的键值对信息,如{username: 'paolino'}|
|$route.query   |请求参数，如/foo?user=1获取到query.user = 1|
|$route.router  |所属路由器以及所属组件信息|
|$route.matched |数组，包含当前匹配的路径中所包含的所有片段所对应的配置参数对象。|
|$route.name    |当前路径名字|

__路由选项__

|路由选项名| 默认值|    作用|
|---------------|:-------------------:|:-------------------------------:|
|hashbang   |true   |将路径格式化为#!开头|
|history    |false  |启用HTML5 history模式，可以使用pushState和replaceState来管理记录|
|abstract   |false| 使用一个不依赖于浏览器的浏览历史虚拟管理后端。|
|transitionOnLoad|  false   |初次加载是否启用场景切换|
|saveScrollPosition|    false|  在启用html5 history模式的时候生效，用于后退操作的时候记住之前的滚动条位置|
|linkActiveClass    |"v-link-active"    |链接被点击时候需要添加到v-link元素上的class类,默认为active|

如，我想采用一个有路径格式化并启用Html5 history功能的路由器，则可以在路由器初始化的时候，指定这些参数:

```javascript
var router = new VueRouter({
  hashbang: true,
  history: true
});
```

请暂时参考这篇文章[VueJs路由跳转——vue-router的使用](http://www.jianshu.com/p/cb918fe14dc6#)  


__注意项__

- 响应路由参数的变化(组件内部跳同一个组件)
- 嵌套路径的 children 
- 编程式的导航  

  1、声明式：router-link :to="..." || 编程式：router.push(...)     
     这个方法会向 history 栈添加一个新的记录
     
```javascript

      // 字符串  
      router.push('home')

      // 对象
      router.push({ path: 'home' })

      // 命名的路由
      router.push({ name: 'user', params: { userId: 123 }})

      // 带查询参数，变成 /register?plan=private
      router.push({ path: 'register', query: { plan: 'private' }})
      
```
    
2、声明式：router-link :to="..." replace || 编程式：router.replace(...)   
    不会向 history 添加新记录，替换掉当前的 history 记录
  
3、router.go(n)  
  
```javascript

    // 在浏览器记录中前进一步，等同于 history.forward()
    router.go(1)

    // 后退一步记录，等同于 history.back()
    router.go(-1)

    // 前进 3 步记录
    router.go(3)

    // 如果 history 记录不够用，那就默默地失败呗
    router.go(-100)
    router.go(100)
    
```
   
4、操作 History  
   
     你也许注意到 router.push、 router.replace 和 router.go 跟 window.history.pushState、 window.history.replaceState 和 window.history.go好像， 实际上它们确实是效仿 window.history API 的。 还有值得提及的，vue-router 的导航方法 （push、 replace、 go） 在各类路由模式（history、 hash 和 abstract）下表现一致。
  
- 命名路由 和 命名视图
  命名路由就是在路由加个 name 的标签  
  命名视图就是在视图加个 name 的标签，可以设置默认值  
  
- 重定向 和 别名  
  重定向：内部跳转 

```javascript 
    { path: '/a', redirect: '/b' }  
    { path: '/a', redirect: { name: 'foo' }}  
    { path: '/a', redirect: to => {
        // 方法接收 目标路由 作为参数
        // return 重定向的 字符串路径/路径对象
    }}
```
  别名：保持当前路由去访问另一个路由  
```javascript
    { path: '/a', component: A, alias: '/b' }
```
- HTML5 History 模式  

  这个给个链接自己去玩 http://router.vuejs.org/zh-cn/essentials/history-mode.html   
  因为这个比较复杂和需要后台来配合...    

- 导航钩子（路由ing锁）        
  正如其名，vue-router 提供的导航钩子主要用来拦截导航，让它完成跳转或取消。有多种方式可以在路由导航发生时执行钩子：全局的, 单个路由独享的, 或者组件级的。
  来来来：http://router.vuejs.org/zh-cn/advanced/navigation-guards.html   

- 路由元信息 （就是标志过滤处理）
- 过渡动效 ... 
- 数据获取      
  1、导航完成之后获取：先完成导航，然后在接下来的组件生命周期钩子中获取数据。在数据获取期间显示『加载中』之类的指示。   
  2、导航完成之前获取：导航完成前，在路由的 enter 钩子中获取数据，在数据获取成功后执行导航。
- 滚动行为      
  注意: 这个功能只在 HTML5 history 模式下可用。
- 路由懒加载    
  说白了就是模块化加载   
  结合 Vue 的 异步组件 和 Webpack 的 code splitting feature, 轻松实现路由组件的懒加载。  


#### Vuex 状态管理  
  
  登录状态，信息储存、暂存，操作状态。
  
  有空可以去解读源码：http://vuex.vuejs.org/en/index.html   
  有中文版： https://segmentfault.com/a/1190000007108052  

#### Vue-loading

#### Vue-resource

#### 动画过渡

  http://vuejs.org/guide/transitions.html  


#### 项目目录

#### webpack

### Vue-cli 构建工具

```v
npm install -g vue-cli  
vue init webpack my-project
```


### 还需要一些周边知识

Node+npm Es6 Sass webpack gulp   

### 项目 

 [豆瓣API](https://developers.douban.com/wiki/?title=guide) [聚合数据](https://www.juhe.cn) 




















