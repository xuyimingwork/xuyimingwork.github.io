---
title: "获取 js 函数参数名"
date: 2019-08-23T11:14:27
categories:
  - 前端
tags:
  - javascript
---

代码来源：[How to get function parameter names/values dynamically?](https://stackoverflow.com/questions/1007981/how-to-get-function-parameter-names-values-dynamically)

```js
/**
 * 获取某方法传入参数的名称列表
 * @param {Function} func 
 * @returns {Array}
 */
function getFuncArguNames(func) {
  const STRIP_COMMENTS = /((\/\/.*$)|(\/\*[\s\S]*?\*\/))/mg;
  const ARGUMENT_NAMES = /([^\s,]+)/g;
  const fnStr = func.toString().replace(STRIP_COMMENTS, '');
  const result = fnStr.slice(fnStr.indexOf('(') + 1, fnStr.indexOf(')')).match(ARGUMENT_NAMES);
  if (result === null) return [];
  return result;
}
```

目前，js 中是没有原生获取函数形参名的方法的，但是将函数 `toString` 后，可以获取到函数的定义代码字符串。通过筛选定义字符串，可以过滤出形参列表。上面的方法就是这么做的。当然这种方法有一定的限制，但对于一般函数、包括箭头函数，都是 OK 的。

注意点
- 若在生产环境上有使用代码优化、进行变量名混淆。可能不适宜使用该方法动态获取形参名。
- 代码来源的连接里还有升级版的方法，用于处理 es6 的形参存在默认值定义时的一些问题。

> 补充：获取函数参数名通常是为了依据参数名动态注入参数，但在混淆的情况无法达成。angularjs 中为此提供的方案是传入数组，数组的最后一项为函数，之前项为字符串，致命要注入到最后项函数中的数据。