---
title: "查看与更改 DOM"
date: 2019-08-20T17:39:19
categories:
  - 前端
tags:
  - chrome
  - 系列：chrome devtools
---

# 查看 DOM 节点

- 在视图上右击并选择检查 => 打开元素面板，高亮选中元素
- 点击开发者工具左上角的选择工具
- 通过上下键浏览节点，左右键展开、折叠节点
- 右击节点（空白处，点击节点文字可能会无所需选项），选择 scroll into view 可以使视图定位至节点位置
- 在元素面板内使用 `Command+F (Mac)` 调出搜索框搜索节点

# 编辑节点

- 双击节点的文字或节点标签编辑对应内容
- 拖拽以调整节点位置
- 右击节点，在 `Force state` 中选择强制节点状态
- 右击节点，选择删除或隐藏

# 在控制台访问节点

- 在元素面板选中节点，节点后出现 `== $0`，此时，在控制台内输入 `$0` 即可访问选中节点
- 如果需要多次访问该节点，可以右击节点，选择 `store as global variable`
- 右击节点，`Copy` => `Copy JS path`，得到能获得该节点的 js 语句，常用于自动化测试

# 断点

- 右击节点，`Break on` => `...` 可以选择当 js 对节点属性变更、移除节点或节点子元素发生更改时触发断点。

# 参见

[Get Started With Viewing And Changing The DOM](https://developers.google.com/web/tools/chrome-devtools/dom/)
