---
title: "禁用 npm version 默认的 git 提交与 git tag 标记"
date: 2019-08-23T10:51:25
categories:
  - 前端
tags:
  - npm
  - node
  - 工程化
---

之前发布版本总是手动更改 npm package 文件，然后提交，然后手动打版本号标记——有点麻烦，就想着优化下构建流程。

由于我目前项目用的是 framework7 框架，在阅读该框架的发布脚本中我发现了 `npm version` 这个命令。

实际上使用这个命令来更新 `package.json` 的版本号才是包升级的正确打开方式。

执行 `npm version x.x.x` 就会用 `x.x.x` 替换掉 `package.json` 中当前的版本，并且如果你项目中使用了 git，该命令还会自动提交一个 commit 并打上一个版本号 `x.x.x` 的 tag。

但在实际的构建流程中，你可能想要先更新 package，然后执行构建，完成构建后有可能产生了一些文件，所以你想在这个时候再提交 git 并打 tag，所以你需要这样使用该命令 `npm --no-git-tag-version version x.x.x`。

这样就可以禁用 `npm version` 默认的 git 行为啦。

# 构建（发布）脚本

```bash
#!/bin/bash
set -e

# 读取新版本
if [[ -z $1 ]]; then
  echo "当前版本：$npm_package_version"
  echo -n "请输入新版本："
  read -r VERSION
else
  VERSION=$1
fi

# 新版本确认
read -p "发布 $VERSION - 确定？(y/n) " -n 1 -r
echo
if [[ $REPLY =~ ^[Yy]$ ]]; then  
  # 更新本地包版本，且不进行提交
  npm --no-git-tag-version version "$VERSION"

  # 执行构建
  npm run build prod

  # git 暂存文件
  git add -A
  # git 提交更新
  git commit -m "$VERSION"
  # git 打 tag
  git tag v"$VERSION"

  echo "release done."
fi
```

# 参见

- [(npm-version) Ability to run npm version without automatically git-committing and tagging](https://github.com/npm/npm/issues/7186)
- [npm-version](https://docs.npmjs.com/cli/version.html)
- [有赞 vant 发布脚本](https://github.com/youzan/vant/blob/dev/build/release.sh)