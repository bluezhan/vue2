this 是 JavaScript 中的一个关键字，当一个函数被调用时，除了传入函数的显式参数以外，名为 this 的隐式参数也被传入了函数。this 参数指向了一个自动生成的内部对象，这个内部对象被称为函数上下文。与其他面向对象的语言不同的是， JavaScript 中的 this 依赖于函数的调用方式。所以，想要明白 this 的指向问题，还必须先研究函数在 JavaScript 中是如何被调用的。

- 上下文调用
- 作用域块

谁调用this，this就指向谁。

上网找关于JavaScript的this一大把，哪里都有。  

### 平台发布

个人网站 github
sf.gg 简书 cnblog 知乎

吐的吞的都在这里

===========================================================

## Vue2.x 坑坑洼洼

#### Vue2.x 坑坑洼洼(一) - 本想搭了个建 

1、独立构建-VS-运行时构建

`Failed to mount component: template or render function not defined. (found in root instance) ` 这是什么错误呢？  
按照vue1.0的配置了webpack后，会出现的错误；这里涉及到vue2.0与vue1.0的第一个不同的地方。具体区别独立构建 vs 运行时构建。在官网的API里知道，是模板编译渲染的问题，解决方法为在webpack配置文件中添加如下配置项：

```
    resolve: {
      alias: {
        'vue$': 'vue/dist/vue.2.min.js'
      }
    }
```

挂载的坑 http://blog.csdn.net/wmwmdtt/article/details/53589662


#### Vue2.x 坑坑洼洼(二) - 过滤器爽过了不能再爽了 

2、过滤器

在vue2.x里，废弃自带过滤器了，想用得自己定义了。
http://www.jianshu.com/p/29b7eaabd1ba


#### Vue2.x 坑坑洼洼(三) - github层出不穷的高仿项目

https://www.zhihu.com/question/38213423


#### 来个Vue2.x版 http://qingmang.me/


## 目录

http://www.jianshu.com/p/dc5057e7ad0d

#### 一、Vue2.x 基础基础

http://www.imooc.com/article/14438  
http://www.open-open.com/lib/view/open1491030239567.html  
http://www.jianshu.com/p/5ba253651c3b  
https://zhuanlan.zhihu.com/p/23078117  
http://www.cnblogs.com/lhb25/p/vue-turtoials-for-new-starter.html  
http://blog.csdn.net/sinat_17775997/article/details/60580780  

vue-devtools、搭建Vue脚手架（vue-cli）、实战案例
自定义便签、todolist、购物车

vue-cli中配置sass和利用SASS函数功能实现px转rem

#### 一、Vue2.x 指令

v-model,v-html,v-text,v-bind,v-if,v-show,v-for,v-on:click,组件,过滤器

#### 二、Vue1.x和Vue2.x的区别和填坑

到了Vue2.x有哪些变化？—— 知识点
到了Vue2.x有哪些变化？—— 组件通信

#### 三、Vue2.x 动画处理

#### 三、Vue2.x 插件和组件

#### 四、Vue2.x 提高熟练程度

Vue2.0生命周期和钩子函数的一些理解  ===》推荐
用webpack（2.x语法）手动搭建Vue项目
全面解析vue-cli生成的项目中使用其他库（js库、css库）
Vuex从入门到入门  ===》 大中型项目复杂逻辑会用到
Vuex从入门到熟练使用
Vuex从入门到熟练使用
vue与服务端通信—vue-resource axios
vue开发过程中跨域最简单解决方案
富文本编辑器Ueditor如何在Vue中使用？

#### 五、Vue2.x 和 React 的区别

http://mp.weixin.qq.com/s/yvQzlTdCVa8OFSi1ULI7-A

#### Vue2.x 番外篇

mixin weex
内存泄漏

#### 结合 Vue2.x 开源项目汇总

http://mp.weixin.qq.com/s/RnBA1OYWCHHzH7Vr8AaTtA

#### Vue2.x 在服务器渲染和搭建

ssr

#### Vue2.x 性能优化


#### 五、ES6基础系列

ES6入门（一）
ES6快速入门（二）
ES6快速入门（三）

http://www.cnblogs.com/Wayou/p/es6_new_features.html


