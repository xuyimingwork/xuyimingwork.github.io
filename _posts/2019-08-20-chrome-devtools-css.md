---
title: "查看与更改 CSS"
date: 2019-08-20T19:39:22
categories:
  - 前端
tags:
  - chrome
  - 系列：chrome devtools
---

# 查看元素的 CSS

开发者工具的元素面板的样式标签页会显示当前 DOM 树中选中的元素的 CSS 规则

![](/assets/images/2019-08-20-chrome-devtools-css.png)

# 给元素添加 CSS 声明

在样式标签页顶部的 `element.style` 内添加，添加完成后会在 DOM 树中看到上步的添加被作为内联样式添加到元素上

![](/assets/images/2019-08-20-chrome-devtools-css-add.png)

# 给元素添加 CSS 类

选中元素，在样式标签中点击 `.cls`，即可为当前元素添加 CSS 类，并启、禁用当前元素已有的 CSS 类

# 给元素添加 CSS 伪类

选中元素，在样式标签中点击 `.hov`，勾选需要添加的伪类

# 更改元素的尺寸

选中元素，在样式标签的盒模型图中更改需要的尺寸

# 参见

[Get Started With Viewing And Changing CSS](https://developers.google.com/web/tools/chrome-devtools/css/#view)
