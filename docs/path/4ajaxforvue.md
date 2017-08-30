## Vue.js应用的四种AJAX请求数据模式

>翻译原文出处：[4 AJAX Patterns For Vue.js Apps](https://vuejsdevelopers.com/2017/08/28/vue-js-ajax-recipes/)  
脾人翻译略差，别见笑。

如果您闲的没事干去两个Vue.js开发人员：“在Vue应用中使用AJAX的正确姿势是咋样的？”，您将会得到三种或更多的不同回答。

Vue.js官方没有提供实现AJAX的指定方式，并且有许多不同的设计模式可以被有效地使用。每个都有自己的利弊，应根据要求进行判断。你甚至可以同时使用几个！

在本文中，我将向您展示您可以在Vue应用程序中实现AJAX的四个位置：

1、[根实例](#根实例)
2、[组件Components](#组件Components)
3、[Vuex actions](#Vuex-actions)
4、[路线导航卫士](#路线导航卫士)
5、[另加：奖金模式]()

我将解释每个方法，举一个例子，并涵盖利弊。

### 一、根实例

在使用Vue框架时，您可以一开始就从根实例发出所有AJAX请求，即写好所有的数据请求，并将所有状态存储在该实例中。如果任何子组件需要数据，它将会顺着`props`中传下来。如果子组件需要刷新数据，则将使用自定义事件来提示根实例请求它。

```js

new Vue({
  data: {
    message: ''
  },
  methods: {
    refreshMessage(resource) {
      this.$http.get('/message').then((response) {
        this.message = response.data.message;
      });
    }
  }
})

Vue.component('sub-component', {
  template: '<div>{{ message }}</div>',
  props: [ 'message' ]
  methods: {
    refreshMessage() {
      this.$emit('refreshMessage');
    }
  }
});

```

__优点__

* 将所有的AJAX逻辑和数据保存在一个地方。
* 保持您的组件“独立性”，以便它们可以更加专注于展示。

__缺点__

随着您的应用扩展，需要书写大量的“props”和自定义事件。

### 二、组件Components

在使用Vue框架时，组件负责管理自己的AJAX请求和独立状态。实际上，您可能需要创建几个“容器组件”来管理本地组“展示组件”的数据。

例如，filter-list可能是一个容器组件包装filter-input和filter-reset，它们作为展示组件。filter-list将包含AJAX数据逻辑，并且将管理该组中所有组件的数据，通过`props`和事件进行通信。

>请参阅[译文《容器组件和展示组件》原作者：Dan Abramov](http://www.jianshu.com/p/6fa2b21f5df3)，可以更好地深入了解这种模式。

为了简化此架构的实现，您可以将任何AJAX逻辑抽象为混合，然后在组件中使用mixin使其成为AJAX。

```js

let mixin = {
  methods: {
    callAJAX(resource) {
      //...
    }
  }
};

Vue.component('container-comp', {
  // No meaningful template, I just manage data for my children
  template: '<div><presentation-comp :mydata="mydata"></presentation-comp></div>', 
  mixins: [ myMixin ],
  data() {
    return {
     //... 
    }
  },

});

Vue.component('presentation-comp', {
  template: '<div>I just show stuff like {{ mydata }}</div>',
  props: [ 'mydata' ]
});

```

__优点__

* 保持组件脱钩和可重用。
* 在任何时候和任何地点都可以获取数据。

__缺点__

* 与其他组件或组件组不通信数据。
* 组件可能会产生很多累赘的职责和重复的功能。

### 三、Vuex actions

在使用Vue框架时，您可以在Vuex `store`中管理状态和AJAX逻辑; 组件可以通过`dispatch`方法操作来请求新数据(store.dispatch将用于触发actions动作)。

如果您要使用此模式，最好从您的`action`中返回一个`promise`，以便对AJAX请求的解析做出反应，例如隐藏加载微调器，重新启用按钮等。

```js

store = new Vuex.Store({
  state: {
    message: ''
  },
  mutations: {
    updateMessage(state, payload) {
      state.message = payload
    }
  },
  actions: {
    refreshMessage(context) {
      return new Promise((resolve) => {
        this.$http.get('...').then((response) => {
          context.commit('updateMessage', response.data.message);
          resolve();
        });
      });
    }
  }
});

Vue.component('my-component', {
  template: '<div>{{ message }}</div>',
  methods: {
    refreshMessage() {
      this.$store.dispatch('refeshMessage').then(() => {
        // do stuff
      });
    }
  },
  computed: {
    message: { return this.$store.state.message; }
  }
});

```


本人比较喜欢这种数据请求模式，因为它很好地分离了你的状态和表现的逻辑。如果你正在使用Vuex，这是要走的路。如果你不使用Vuex，这模式可能是一个很好的理由。

__优点__

* 所有的根组件架构的优点，不需要`props `和自定义事件。

__缺点__

* 增加Vuex的开销。


### 四、路线导航卫士

在使用Vue框架时，您的应用程序分为多个页面，当路由更变时，将抓取页面及其子组件所需的所有数据。

这种方法的主要优点是它真正简化了您的UI。如果组件独立获取自己的数据，则当组件数据以任意顺序填充时，页面将不可预测地重新呈现。

实现这一点的一个整洁的方法是在您的服务器上为每个页面创建端点，例如/about，/contact与您的应用程序中的路由名称相匹配。然后，您可以实现一个通用beforeRouteEnter钩子，将所有数据属性合并到页面组件的数据中：


```js

import axios from 'axios';

router.beforeRouteEnter((to, from, next) => {
  axios.get(`/api${to.path}`).then(({ data }) => {
    next(vm => Object.assign(vm.$data, data))
  });
})

```


__优点__

* 使UI更可预测。

__缺点__

* 总体来说，直到所有数据准备就绪了 ,页面才能呈现。
* 如果不使用路线，这模式没有太多的帮助。

### 奖金模式：将第一个AJAX调用的服务器渲染到页面中

建议在初始页面加载时使用AJAX来检索应用程序状态，因为它需要额外的往返服务器，这将延迟应用程序的渲染。

相反，将初始应用程序状态注入HTML页面的内联脚本中，以便应用程序作为全局变量在需要时可用。

```html
<html>
...
<head>
  ...
  <script type="text/javascript">
   window.__INITIAL_STATE__ = '{ "data": [ ... ] }';
  </script>
</head>
<body>
  <div id="app"></div>
</body>
</html>

```

然后，AJAX可以更适合地用于后续数据提取。

如果您有兴趣了解有关此架构的更多信息，请参阅作者的文章“[避免此全面堆栈Vue / Laravel应用程序中的常见反模式](https://vuejsdevelopers.com/2017/08/06/vue-js-laravel-full-stack-ajax/)”。