---
title: "使用脚本创建博文模板"
date: 2019-08-22T11:58:54
categories:
  - 博客
tags:
  - node 脚本
  - 模板创建
---

因 `jekyll` 要求，新建博文需要在 `_posts` 文件夹下，文件以日期开头，并在顶部有固定模板。为优化创建博文体验，想使用 JavaScript 脚本来完成这一过程。

使用 JavaScript 编写脚本创建文件的核心关键点在于：

- 使用 node 执行 JavaScript 脚本

在 node 包里，使用 node 执行 JavaScript 脚本的操作可以统一放到 npm 脚本中统一入口，本例中我的脚本位于 `scripts/new-post.js`，npm 脚本则为 `"new": "node scripts/new-post.js"`，执行时则需要在命令行输入 `npm run new post name` 即可。

- 脚本接收命令行传入参数

用 node 执行 JavaScript 脚本时，参数可以通过 `process.argv` 获取，该数组即为运行命令时传给 shell 的参数。但为了后期方便给脚本添加参数，我引入 `yargs` 库来简化参数获取，`yargs` 的[具体使用方式](https://github.com/yargs/yargs)。

当下，只需：

```js
const argv = require('yargs').argv;
```

而后，便可从 `argv` 中获取想要参数，具体而言，本次是从 `argv._` 中获取数据。

- 写入文件

写入文件的核心是使用 node 库 `fs` 的 `writeFile()` 方法，当然还会涉及到判断文件是否存在，文件的父目录是否存在等。此处，我将该方法包裹在了 Promise 中。

```js
function create(file, content) {
  return new Promise((resolve, reject) => {
    fs.writeFile(file, content ? content : '', err => {
      if (err) return reject(err);
      console.log(`create file ${file}`);
      resolve();
    })
  })
}
```

完整代码见本仓库的 `scripts/new-post.js` 文件。

# 参见

- [Node.js实战（第2版）第 11 章　编写命令行程序](http://www.ituring.com.cn/book/tupubarticle/23095)
- [Vue 构建脚本](https://github.com/vuejs/vue/blob/dev/scripts/build.js)
