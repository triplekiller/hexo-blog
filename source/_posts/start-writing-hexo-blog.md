---
title: Start Writing Hexo Blog
date: 2018-01-23 15:04:19
tags: [hexo]
categories: Network
---

Hexo博客已经搭建完毕，但有时需要在不同的电脑上写博客，需要重新安装Hexo博客环境，具体步骤如下：

### Clone博客源码

``` bash
$ git clone https://github.com/triplekiller/hexo-blog.git
$ cd hexo-blog
$ git submodule update --init
$ git config user.name "Ethan Xia"
$ git config user.email "sanhust@gmail.com"
```

### 安装Hexo

根据[Hexo官方文档](https://hexo.io/zh-cn/docs/index.html)，安装Hexo及其依赖的Node.js等环境。

* 安装hexo-cli，安装路径在/usr/lib/node_modules/hexo-cli/中

`$ npm install -g hexo-cli`

* 安装hexo及package.json中指定的依赖包，安装路径在node_modules目录中

`$ npm install --save`

* 安装hexo-server（Hexo 3.0后hexo-server需要单独安装）

`$ npm install hexo-server --save`

### 本地预览

生成静态文件，路径在public目录中

`$ hexo g`

启动服务器，默认访问地址为：http://localhost:4000/

`$ hexo s`

### 写博客

`$ hexo new post start-writing-hexo-blog`
