---
title: "HTML 对比 DOM"
date: 2019-08-20T09:24:08
categories:
  - 前端
tags:
  - html
  - dom
---

当你请求某个页面，服务端返回 HTML，浏览器解析该 HTML 并创建对应的对象树（或节点数），该对象树即为 DOM，代表当前页面内容，此时，DOM 与 HTML 一致。

当使用 JavaScript 对 DOM 进行增、删、改之后，DOM 与 HTML 就不再一致。

简单来说，HTML 代表最初的页面内容，DOM 代表当前的页面内容。在 JavaScript 操作 DOM 后，DOM 即与 HTML 不同。

# 参见

[Appendix: HTML versus the DOM](https://developers.google.com/web/tools/chrome-devtools/dom/#appendix)
