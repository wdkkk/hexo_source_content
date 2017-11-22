---
title: yarn简单使用
date: 2017-11-21 10:59:00
tags:
---

​      由于npm 在安装包的过程中会出现各种奇奇怪怪的问题，于是就出现了替代品yarn。yarn作为一个包管理器，有许多优点，比如速度快、支持离线安装、支持npm包等，它的使用也很简单。

<!-- more -->

1. 安装。在官网下载安装包即可，如Windows平台直接下载msi格式的安装文件安装即可，当然前提是安装Node.js

2. 使用：

- 初始化项目

```bash
yarn init
```

- 添加一个包

```bash
yarn add [package]
yarn add [package]@[version]
yarn add [package]@[tag]
```

- 更新包

```bash
yarn upgrade [package]
yarn upgrade [package]@[version]
yarn upgrade [package]@[tag]
```

- 删除包

```bash
yarn remove [package]
```

- 安装所有包

```bash
yarn  或者 yarn install
```



