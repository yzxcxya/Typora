# 简单搭建一个vue2项目

## 1. 创建项目

webstorm创建一个vue3项目

## 2. **安装配置依赖**

### vuex

1.src 文件夹下新建文件夹及文件store/index.js，index.js新增以下代码

~~~
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
export default new Vuex.Store({})
~~~

2.main.js中配置

~~~
new Vue({
	store,
	render:h => h(APP)
}).$mount('#app')
~~~

### vue-router

在src 文件夹下新建文件夹及文件router/index.js，在index.js中配置

~~~
import Vue from 'vue'
import VueRouter from 'vue-router'
Vue.use(VueRouter)
const routes = []
const router = new VueRouter({
  mode:"history",
  base:'process.env.BASE_URL',
  routes
})
export default router
~~~

### axios

> axios是一个库，并不是vue中的第三方插件，使用时不能通过Vue.use()安装插件，需要**在原型上进行挂载绑定**

文档：[使用说明 · Axios 中文说明 · 看云 (kancloud.cn)](https://www.kancloud.cn/yunye/axios/234845)

1.安装

~~~
npm install axios -S
npm install vue-axios -S
~~~

2.配置

在main.js中引入

~~~
import Vue from 'vue'
import axios from ‘axios’
Vue.prototype.$http = axios
~~~

使用方法：

~~~
this.$axios.get(url).then((response) => {
  console.log(response.data)
})
~~~

### element-ui

1.安装

~~~
npm install element-ui -S
~~~

2.配置

在main.js中配置

~~~
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(ElementUI);
~~~



### sass

### eslint

## 3. 项目配置