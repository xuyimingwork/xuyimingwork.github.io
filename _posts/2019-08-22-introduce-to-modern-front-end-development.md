---
title: "小谈前端开发"
date: 2019-08-22T15:52:25
categories:
  - 前端
tags:
  - 闲谈
---

# 引入

问：从 jquery 时代到 angularjs 再到当前的 React、Vue、Angular 到底改变了什么？
答：当前的框架帮助我们完成数据与视图间的绑定，至此，数据即视图，视图反应数据，使我们只需关注数据。

问：什么是只关注数据？
答：就是框架会追踪数据的变化，并将数据的变化自动反应在视图上，你对数据的更改即对视图的更改，数据的状态即视图的状态。

由于数据表示视图，开发的问题即变成了：
- 如何管理数据
- 模板绑定哪些数据、模板如何绑定数据

上述问题的关键在于如何管理数据（angularjs 中，可通过服务解决；vue 中，通过 vuex；react 中有 React-Hooks 与 Redux），至于模板绑定数据问题，不同框架有不同模板语法，具体去看即可。此外，还有如何更新数据可以触发框架更新视图的问题，取决于框架的响应式实现。

# 搭建一个 vue 学习环境

- [安装 vs code](https://code.visualstudio.com)
- 安装 vs code Live Server 插件
- 安装 chrome [vue 开发者工具](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd)
- 在 vs code 中创建基础 html 模板并尝试 vue 官网教程（完成至自定义指令(Custom Directive)一节）

> 需要关注 vue 组件、slot、mixin。
> 
> vs code Live Server 是个简单服务器，可以帮你实现热更新，方便你的学习。这种模式和后期 webpack-dev-server 模式很像，有助于你提前体验。

# 关于 node 与 npm

了解 node 如何执行 JavaScript 脚本，了解为何在编写 node 的 JavaScript 脚本时可以使用 `require`、`module`。

> 在 node 8 中，如果在脚本首行故意留下语法错误（如 `const a = ,`），在用 node 执行时即可看见，node 将脚本包裹在一个立即执行函数中，而脚本内使用的 `require`、`module` 等为该立即执行函数传入的参数

理解 npm 脚本，`package.json` 中的 `main`、`scripts`、`dependencies` 等属性。

> 理解 npm 包的语义化版本，理解 `package-lock.json` 的作用。

# webpack 与 babel

- 理解 webpack 作为打包工具的作用，webpack 的入口（entry）、出口（output）、加载器（loader）、插件（plugin）四个关键概念以及他们在 webpack 构建过程中的作用，了解 webpack 模式概念。

- 理解 babel，及其在 webpack 中的角色（loader）。此处可以去 babel 官网体验 babel 转换过程，理解 babel 配置文件中 `presets` 的作用，此处还可前往 [caniuse](https://www.caniuse.com/) 了解浏览器支持哪些特性。

你可以依据 webpack 官网的指南，自行搭建 webpack demo，体验 webpack 的打包流程。此时，你应该基本了解一个项目从 `npm run 命令` 开始大体经历了哪些过程，是时候上手一个实际项目了。

# 实际项目上手

此时，你可以阅读 vue 官网的”单文件组件“一节，并通过 `Vue CLI` 搭建一个初始项目。

> 结合上述，理解 `.vue` 文件是通过 `vue-loader` 变成 vue 可理解的对象传入使用的。

依据需要，学习 vue-router、vuex 的使用。

此时，你还可以尝试搭建、或下载运行一些 vue 项目，如：
- [Element 快速上手](https://element.eleme.io/#/zh-CN/component/quickstart)
- [Ant Design 快速上手](https://vue.ant.design/docs/vue/getting-started-cn/)
- [基于 vue2 + vuex 构建一个具有 45 个页面的大型单页面应用](https://github.com/bailicangdu/vue2-elm)

# 参见

- [jquery 与 angularjs 实现 hello world 对比](https://www.youtube.com/watch?time_continue=137&v=uFTFsKmkQnQ)
- [新手向：Vue 2.0 的建议学习顺序](https://zhuanlan.zhihu.com/p/23134551)

