---
title: npm安装中的问题
date: 2017-11-21 10:33:48
tags:
---

使用npm安装包时由于各种客观原因有时安装速度太慢，可使用淘宝镜像解决<!-- more -->

使用方式有两种：

1. 修改npm的镜像源

   ```bash
   npm config set registry https://registry.npm.taobao.org
   ```

   修改之后通过下面的命令查看

   ```bash
   npm get registry	
   ```

   还原为npm官方源

   ```bash
   npm config set registry https://registry.npmjs.org/
   ```

   ​

2. 使用cnpm

   ```bash
   npm install -g cnpm --registry=https://registry.npm.taobao.org
   ```

   全局安装cnpm替换npm，使用方式：

   ```bash
   cnpm install [name]
   ```

   ​