---
layout: post
title: vue环境安装和项目结构介绍
date: 2021-5-5
categories: [Vue]
tags: [Vue]
excerpt: vue环境安装和项目结构介绍
---

# 基本环境搭建

首先需要安装两个东西：

1. NodeJS
2. npm

直接搜索下载 NodeJS 即可，安装成功之后，npm 也就有了。安装成功之后，可以 在 cmd 命令哈验证是否安装成功：

```bash
D:\>node -v
v14.16.1
D:\>npm -v
6.14.12
```

NodeJS 安装成功之后，接下来安装 Vue的工具：

```bash
npm install -g vue-cli   # 只需要第一次安装时执行
vue init webpack my-project  # 使用webpack模板创建一个vue项目
cd my-project #进入到项目目录中
npm install  # 下载依赖（如果在项目创建的最后一步选择了自动执行npm install，则该步骤可以省略）
npm run dev # 启动项目
```

启动成功后，浏览器输入  `http://localhost:8080` 查看页面

**执行 `npm install` 命令时，默认使用的是国外的下载源 ，可以通过如下代码配置为使用淘宝的镜像：**

```bash
npm config set registry https://registry.npm.taobao.org
```

<br/>

# Vue 项目结构介绍

Vue 项目创建完成后，使用 Web Storm 打开项目，项目目录如下：

```text
.
|-build
|-config
|-node_modules
|-src
|-static
|-.babelrc
|-.editorconfig
|-.gitignore
|-.postcssrc.js
|-index.html
|-package.json
|-package-lock.json
|-README.md
```

1. build 文件夹，用来存放项目构建脚本
2. config 中存放项目的一些基本配置信息，最常用的就是端口转发
3. node_modules 这个目录存放的是项目的所有依赖，即 npm install 命令下载下来的文件
4. src 这个目录下存放项目的源码，即开发者写的代码放在这里
5. static 用来存放静态资源
6. index.html 则是项目的首页，入口页，也是整个项目唯一的HTML页面
7. package.json 中定义了项目的所有依赖，包括开发时依赖和发布时依赖

对于开发者来说，以后 99.99% 的工作都是在 src 中完成的，src 中的文件目录如下：

```text
|-src
||-assets
||-components
||-router
||-App.vue
||-main.js
```

1. assets 目录用来存放资产文件
2. components 目录用来存放组件（一些可复用，非独立的页面），当然开发者也可以在 components 中直接创建完整页面。
3. 推荐在 components 中存放组件，另外单独新建一个 page 文件夹，专门用来放完整页面。
4. router 目录中，存放了路由的js文件
5. App.vue 是一个Vue组件，也是项目的第一个Vue组件
6. main.js相当于Java中的main方法，是整个项目的入口js

main.js 内容如下：

```js
import Vue from 'vue'
import App from './App'
import router from './router'
Vue.config.productionTip = false
/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})
```

1. 在main.js 中，首先导入 Vue 对象
2. 导入 App.vue ，并且命名为 App
3. 导入router，注意，由于router目录下路由默认文件名为 index.js ，因此可以省略
4. 所有东西都导入成功后，创建一个Vue对象，设置要被Vue处理的节点是 ‘#app’，’#app’ 指提前在index.html 文件中定义的一个div
5. 将 router 设置到 vue 对象中，这里是一个简化的写法，完整的写法是 router:router，如果 key/value 一模一样，则可以简写。
6. 声明一个组件 App，App 这个组件在一开始已经导入到项目中了，但是直接导入的组件无法直接使用，必须要声明。
7. template 中定义了页面模板，即将 App 组件中的内容渲染到 ‘#app’ 这个div 中。

项目启动成功后，看到的页面效果定义在 App.vue 中

```vue
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-view/>
  </div>
</template>
<script>
export default {
  name: 'App'
}
</script>
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

1. App.vue 是一个vue组件，这个组件中包含三部分内容：1.页面模板（template）；2.页面脚本（script）；3.页面样式（style）
2. 页面模板中，定义了页面的 HTML 元素，这里定义了两个，一个是一张图片，另一个则是一个 router-view
3. 页面脚本主要用来实现当前页面数据初始化、事件处理等等操作
4. 页面样式就是针对 template 中 HTML 元素的页面美化操作

需要额外解释的是，router-view，这个指展示路由页面的位置，可以简单理解为一个占位符，这个占位符展示的内容将根据当前具体的 URL 地址来定。具体展示的内容，要参考路由表，即 router/index.js 文件，该文件如下：

```vue
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
Vue.use(Router)
export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld
    }
  ]
})
```

1. 这个文件中，首先导入了Vue对象、Router对象以及 HelloWorld 组件，

2. 创建一个Router对象，并定义路由表

3. 这里定义的路由表，path为 `/` ，对应的组件为 HelloWorld，即浏览器地址为 `/` 时，在router-view位置显示 HelloWorld 组件

   <br/>

# 项目编译

这么大一个前端项目，肯定没法直接发布运行，当开发者完成项目开发后，将 cmd 命令行定位到当前项目目录，然后执行如下命令对项目进行打包：

```
npm run build
```

打包成功后，当前项目目录下会生成一个 dist 文件夹，这个文件夹中有两个文件，分别是 index.html 和 static ，index.html 页面就是我们 SPA 项目中唯一的 HTML 页面了，static 中则保存了编译后的 js、css等文件，项目发布时，可以使用 nginx 独立部署 dist 中的静态文件，也可以将静态文件拷贝到 Spring Boot 项目的 static 目录下，然后对 Spring Boot 项目进行编译打包发布。
[参考](http://www.javaboy.org/2019/0419/springboot-vue.html)
