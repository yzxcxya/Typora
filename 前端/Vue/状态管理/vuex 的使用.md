# vuex 的使用

[vuex4官方文档](https://vuex.vuejs.org/zh/)

### 安装

```
npm install vuex@next -S //vuex3不需要加@next
或
yarn add vuex@next -S
```

### 快速开始

1. 创建一个store仓库

   > store”基本上就是一个容器，它包含着你的应用中大部分的**状态 (state)**。Vuex 和单纯的全局对象有以下两点不同:
   >
   > 1.Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。
   >
   > 2.你不能直接改变 store 中的状态。改变 store 中的状态的唯一途径就是显式地**提交 (commit) mutation**。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。

   ```js
   import { createApp } from 'vue'
   import { createStore } from 'vuex'
   // 创建一个新的 store 示例
   const store = createStore({
   	state() {
   		return {
   			count :0,
   		}
   	},
   	mutations: {
   		increment (state) {
               state.count++
           }
   	}
   })
   const app = createApp({ /*根组件*/ })
   app.use(store)
   ```
   
   在 vue 组件中，通过 `this.$store` 访问store实例。
   
   ```js
   methods: {
       increment() {
           this.$store.commit('increment')
           console.log(this.$store.state.count)
       }
   }
   ```

### State

访问state的几种方式

1.`this.$store.state.属性名` 或 `this.$store.state.模块名.属性名`

2.通过辅助函数mapState

```
1.导入辅助函数
import { mapState } from 'vuex'
2.在 computed 里引入
...mapState({属性名:state=>state.属性名})
或
...mapState(['属性名1','属性名2'])
或
...mapState('模块命名空间名',['属性名1','属性名2'])
```

### Getter

有时候我们需要从 store 中的 state 中派生出一些状态，例如对列表进行过滤并计数：“getter” 可以认为是 store 的计算属性

在 store/index.js 中定义

通过属性访问

```
getters: {
	// Getter 接受 state 作为其第一个参数
    doneTodos: (state) => {
      return state.todos.filter(todo => todo.done)
    },
    // Getter 也可以接受其他 getter 作为第二个参数
    doneTodosCount (state, getters) {
      return getters.doneTodos.length
    },
  }
```

通过方法访问

```
getters: {
    getTodoById: (state) => (id) => {
      return state.todos.find(item => item.id === id)
    },
  }
```

使用getters的几种方式

1.`this.$store.getters.属性名 `或 `this.$store.getters['模块名/属性名']`

2.使用辅助函数mapGetter

```
1.导入辅助函数
import { mapGetters } from 'vuex'
2.在 computed 里引入
...mapGetters(['属性名1','属性名2'])
或
...mapGetters('模块命名空间名',['属性名1','属性名2'])
```

### Mutation

更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的**事件类型 (type)和一个回调函数 (handler)**。这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数

你不能直接调用一个 mutation 处理函数。这个选项更像是事件注册：“当触发一个类型为 `increment` 的 mutation 时，调用此函数。”要唤醒一个 mutation 处理函数，你需要以相应的 type 调用 **store.commit** 方法

你可以向 `store.commit` 传入额外的参数，即 mutation 的**载荷（payload）**,在大多数情况下，载荷应该是一个对象，这样可以包含多个字段并且记录的 mutation 会更易读

```js
mutations: {
	increment(state,payload) {
        state.count = payload.count
    }
}
```

> Mutation 必须是同步函数

使用mutations的几种方式

1.直接使用

```
this.$store.commit('事件名', 提交载荷)
或
this.$store.commit('模块名/事件名', 提交载荷)
```

2.使用type

```
this.$store.commit({
  type: '事件名',
  提交载荷
})
```

3.使用辅助函数

```
1.导入辅助函数
import { mapMutations } from 'vuex'
2.在 methods 里引入
...mapMutations(['事件名1','事件名2'])
或
...mapMutations('模块名',['事件名'])
3.使用：this.事件名(提交载荷)
```

### Action

Action 类似于 mutation，不同在于：

- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意异步操作。

Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 `context.commit` 提交一个 mutation，或者通过 `context.state` 和 `context.getters` 来获取 state 和 getters

```js
const store = createStore({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context,payload) {
      context.commit('increment')
    }
    或者
    increment ({ commit,state },payload) {
      commit('increment')
    }
  }
})
```

使用方式：

1.分发Action

```
this.$store.dispatch('事件名', 提交载荷)
或
this.$store.dispatch('模块名/事件名', 提交载荷)
```

2.使用辅助函数mapActions

```
1.导入辅助函数
import { mapActions } from 'vuex'
2.在 methods 里引入
...mapActions(['事件名1','事件名2'])
或
...mapActions('模块名',['事件名'])
3.使用：this.事件名(提交载荷)
```

### Module

Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割

```
// 导入子模块
import area from './module/area.js'
// createStore 对象里加上modules
modules: {
    area,
  }
  
// 子模块导出
export default {
    namespaced:true,
    state() {
        return {
        }
    },
    getters: {
    },
    mutations: {
    },
    actions: {
    },
}
```

