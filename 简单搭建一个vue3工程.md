# 简单搭建一个vue3工程

### 1.创建项目

可以利用`vue ui`命令或者webstorm创建一个vue3项目

> 用`vue ui`可视化界面创建项目前，node下应该全局安装了vue构建工具的脚手架vue cli
>
> 安装命令为：`npm install @vue/cli -g`，此命令会下载最新的vue3的脚手架。如果创建的是vue2的项目，安装命令应该是` npm install vue-cli -g`，不出意外会给你安装最新版本的脚手架，也就是2.9.6版本的。安装完成后用`vue -V`命令查看安装的脚手架版本即可。脚手架的版本很大程度决定了整个工程的配置文件应该怎么配，一般都可以去官网上找找跟以往的不同配置点在哪。

可视化界面中可以预选如下配置：

![image-20220417114242540](https://picture-bucket-1306212000.cos.ap-nanjing.myqcloud.com/markdown/202204171142604.png)

### 2.安装配置依赖

#### vuex

1.安装

~~~
npm install vuex@next -S
~~~

> @next是npm包的一个tag，除此之外还有@beta，@latest等，可以参考博客[npm包tag的使用，以及@beta和@next的含义 - 简书 ](https://www.jianshu.com/p/c45f4eca98de)，@符号后面还可以直接跟版本号

2.配置

在main.js中增加以下

~~~
import store from './store'
createApp(App).use(store)
~~~

在src 文件夹下新建文件夹及文件store/index.js，index.js中新增以下代码：

~~~
import { createStore } from 'vuex'

export default createStore({
  // state参数，数据仓库，用来存储数据的
  state: {
  },
  // 获取数据的，有点像computed的用法
  getters: {
  },
  // 更改state数据的方法都要卸载mutations里
  mutations: {
  },
  // 异步，异步的方法都写在这里，但最后还是需要通过mutations来修改state的数据
  actions: {
  },
  // 分包。Vuex将store分割到模块（module）。每个模块拥有自己的 state、mutation、action、getters
  modules: {
  }
})
~~~

#### vue-router

1.安装

~~~
npm install vue-router -S
~~~

2.配置

在main.js中引入

~~~
import router from './router'
createApp(App).use(router)
~~~

在src 文件夹下新建文件夹及文件router/index.js，在index.js中配置

~~~
import { createRouter, createWebHistory } from 'vue-router'
const routes = []
const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes
})
export default router

// 哈希模式如下

import { createRouter, createWebHashHistory, RouteRecordRaw } from 'vue-router'
const routes: Array<RouteRecordRaw> = []
const router = createRouter({
  history: createWebHashHistory(),
  routes
})
export default router
~~~

#### axios

> axios是一个库，并不是vue中的第三方插件，使用时不能通过Vue.use()安装插件，需要**在原型上进行挂载绑定**

文档：[使用说明 · Axios 中文说明 · 看云 (kancloud.cn)](https://www.kancloud.cn/yunye/axios/234845)

1.安装

~~~
npm install axios -S
npm install vue-axios -S
~~~

2.配置

vue3: 在main.js中引入

~~~
import axios from 'axios'
import VueAxios from 'vue-axios'
createApp(App).use(VueAxios,axios).mount('#app')
~~~

#### element-plus

1.安装

~~~
npm install element-plus -S
~~~

> 注意vue3后不应该再使用element-ui了，应该使用官方为vue3开发的element-plus

2.配置

在main.js中配置

~~~
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
createApp(App).use(ElementPlus)
~~~

#### sass

1.安装

~~~
npm install node-sass sass-loader -D
~~~



#### eslint

1.安装

~~~

~~~

### 3.项目配置

#### vue.config.js

~~~
const { defineConfig } = require('@vue/cli-service')
const path = require("path")
const resolve = (dir) => path.join(__dirname, dir)
module.exports = defineConfig({
// url基本路径，如果配置为/a，则需要访问http://localhost:8081/a
  publicPath: "",
  // 包文件输出路径，即打包到哪里
  outputDir: "dist",
  // 静态资源地址
  assetsDir: 'public',
  // eslint-loader 是否在保存的时候检查
  lintOnSave: false,
  // 生产环境是否生成 sourceMap 文件
  productionSourceMap: false,
  //
  // runtimeCompiler: true,
  //文件hash
  filenameHashing: true,
  // 调整内部的 webpack 配置。
  chainWebpack: (config) => {
    // 配置文件夹别名，template和style里引入的时候前面需要加~，script不需要
    config.resolve.alias
        .set("@", resolve("src"))
        .set("@assets", resolve("src/assets"))
        .set("@components", resolve("src/components"))
        .set("@router", resolve("src/router"))
        .set("@store", resolve("src/store"))
        .set("@views", resolve("src/views"))
        .set("@public", resolve("public"))
  },
  // css相关配置
  css: {
    // css预设器配置项
    loaderOptions: {
      sass: {
        additionalData: `@import './src/assets/style/variables.scss';`,
      },
    },
  },
  devServer: {
    port: 8081,
    https:false,
    host: '0.0.0.0',
    // hot: 'only',
    // useLocalIp: true, // 可以使用localhost和127.0.0.1
    // disableHostCheck: true,  // 绕过主机检查
    compress: true, // 浏览器请求静态资源时压缩一下，打开浏览器的检查时可以看到bundle.js的content-encoding是gzip，浏览器自动解压
    client: {
      webSocketURL: 'ws://0.0.0.0:8081/ws',
    },
    headers: {
      'Access-Control-Allow-Origin': '*',
    },  // 在所有响应中添加首部内容
  },
  transpileDependencies: true
})

~~~

#### babel.config.js



#### jsconfig.json

