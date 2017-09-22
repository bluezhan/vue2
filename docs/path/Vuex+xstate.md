
# Vuex + xstate

>翻译原地址：https://medium.com/@brockreece/vuex-xstate-4f9ea23bb24e

最近看了David Khourshid在`React Rally`上的《关于基于状态机的UI设计》最新演讲，激动不己，决定拿来玩一玩；
项目：[xstate](https://github.com/davidkpiano/xstate)。

PPT:[Infinitely Better UIs with Finite Automata](http://slides.com/davidkhourshid/finite-state-machines#/)

而后，我觉得这种模式非常适合Vue.js，特别是Vuex；我在`codepen`上快速地完成了几个示例；运行并检查了之后，我将这些示例发送给了David Khourshid。

让我们看看`xstate`在Vue是怎么集成的：

```html
<div id="app">
  <div>{{ state }}</div>
  <button :style="{color: state}">{{ buttonText }}</button>
</div>
```

```js
const lightMachine = xstate.Machine({
  key: "light",
  initial: "green",
  states: {
    green: {
      on: {
        TIMER: "yellow"
      }
    },
    yellow: {
      on: {
        TIMER: "red"
      }
    },
    red: {
      on: {
        TIMER: "green"
      }
    }
  }
})
const store = new Vuex.Store({
  state: {
    currentState: lightMachine.initial
  },
  mutations: {
    transition(state, action) {
      state.currentState = lightMachine.transition(state.currentState, action).value
    }
  }
})
new Vue({
  el: "#app",
  store,
  mounted() {
    setInterval(() => {
      this.$store.commit("transition", "TIMER")
    }, 3000)
  },
  computed: {
    buttonText() {
      return this.buttonOptions[this.state]
    },
    state() {
      return this.$store.state.currentState
    }
  },
  data() {
    return {
      buttonOptions: {
        green: "foo",
        yellow: "bar",
        red: "baz"
      }
    }
  }
})

```

https://codepen.io/BrockReece/pen/ZJqgpO

对于Vuex示例，我将状态机逻辑保持在Vuex存储之外，并且仅跟踪存储中的当前状态。我认为这种方法可以非常适合Vuex模块。

```js
new Vue({
  el: "#app",
  mounted() {
    this.state = this.lightMachine.initial;
    setInterval(() => {
      this.state = this.lightMachine.transition(this.state, "TIMER").value;
    }, 3000);
  },

  computed: {
    buttonText() {
      return this.buttonOptions[this.state];
    }
  },
  data() {
    return {
      state: null,
      lightMachine: xstate.Machine({
        key: "light",
        initial: "green",
        states: {
          green: {
            on: {
              TIMER: "yellow"
            }
          },
          yellow: {
            on: {
              TIMER: "red"
            }
          },
          red: {
            on: {
              TIMER: "green"
            }
          }
        }
      }),
      buttonOptions: {
        green: "foo",
        yellow: "bar",
        red: "baz"
      }
    };
  }
});

```


https://codepen.io/BrockReece/pen/EvdwpJ

我想David Khourshid已经把我提供的示例放到`xstate`项目的案例里面了。

![]()

好了，接下来我要认真考虑在下一个项目中狠狠地使用上这个东西。