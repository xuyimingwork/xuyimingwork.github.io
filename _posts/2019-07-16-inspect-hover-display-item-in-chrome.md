---
title: "在 chrome 中检查悬浮样式"
categories:
  - 前端
tags:
  - chrome
---

当你需要调整某个元素 A 的样式，但 A 元素仅在鼠标悬浮于某个 B 元素上才出现时。

- 若鼠标在 A 元素上 A 不消失，可以直接右击 A 元素选择“检查”

- 若 A 元素的悬浮通过 B 元素的 CSS 伪类触发，可选中 B 元素，在开发面板中更改 B 元素的状态
![](/assets/images/2019-07-16-inspect-hover-display-item-in-chrome-css.png)

- 若 A 元素的悬浮通过 B 元素的 js 触发，则可在控制台通过 `$('#menu').trigger('mouseover');` 手动触发 A 悬浮，而后可暂停 JavaScript 执行保持当前状态以调整
![](/assets/images/2019-07-16-inspect-hover-display-item-in-chrome-js.png)

# 参见

- [Inspect Javascript Hover in Chrome Developer Tools](https://stackoverflow.com/questions/25510439/inspect-javascript-hover-in-chrome-developer-tools)

- [How can Chrome's Dev Tools be used to inspect a hover action if the style changes back to normal when moving the cursor back to the Dev Tools window?](https://www.quora.com/How-can-Chromes-Dev-Tools-be-used-to-inspect-a-hover-action-if-the-style-changes-back-to-normal-when-moving-the-cursor-back-to-the-Dev-Tools-window)
