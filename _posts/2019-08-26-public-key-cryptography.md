---
title: "公开密钥加密"
date: 2019-08-26T16:51:33
categories:
  - 加密
tags:
  - 非对称加密
---

公开密钥加密，又称非对称加密。得名于加密、解密过程分别使用不同密钥，加密使用公钥，解密使用密钥。

具体流程：

- 接收方 B 使用算法生成一对密钥 —— 公钥、密钥
- 接收方 B 通过电子证书公开公钥
- 发送方 A 通过证书获取接收方 B 的公钥
- 发送方 A 使用公钥加密明文，得到密文
- 发送发 A 发送密文给接收方 B
- 接收方 B 通过密钥解密密文，得到明文

常用算法：

- [RSA](https://zh.wikipedia.org/wiki/RSA%E5%8A%A0%E5%AF%86%E6%BC%94%E7%AE%97%E6%B3%95)

# 参见

- [Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography)
