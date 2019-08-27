---
title: "HTTP 协议"
date: 2019-08-26T17:44:42
categories:
  - 网络
tags:
  - http
---

- HTTP 1.0
  - 不复用 TCP，即每个 HTTP 请求均会创建一个 TCP 连接

- HTTP 1.1 vs 1.0
  - 允许复用 TCP，但每个 HTTP 请求需要依次获取，即上一个 HTTP 请求完成再发起下一个 HTTP 请求（造成队头阻塞）

- HTTP 2.0 vs 1.1
  - 多路复用（解决 HTTP 队头阻塞问题）


# 参见

- [How does “view source” in response headers go missing for certain sites](https://stackoverflow.com/questions/40800140/how-does-view-source-in-response-headers-go-missing-for-certain-sites)


