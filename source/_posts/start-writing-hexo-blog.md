---
title: Start Writing Hexo Blog
date: 2018-01-23 15:04:19
tags: [hexo]
categories: Network
---

Hexo博客已经搭建完毕，但有时需要在不同的电脑上写博客，需要重新安装Hexo博客环境，具体步骤如下：

## Clone博客源码

``` bash
$ git clone https://github.com/triplekiller/hexo-blog.git
$ cd hexo-blog
$ git submodule update --init
$ git config user.name "Ethan Xia"
$ git config user.email "sanhust@gmail.com"
```

## 安装Node.js

由于Ubuntu 16.04自带的nodejs版本太低(v4.6.2)，不推荐直接使用`sudo apt-get install -y nodejs`安装默认版本。可以使用以下两种方式进行安装。

### 使用[nvm](https://github.com/creationix/nvm)安装

下载nvm到`~/.nvm`

`$ wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh`

添加环境变量到`~/.bashrc`

``` bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
[[ -r $NVM_DIR/bash_completion ]] && \. $NVM_DIR/bash_completion
```

下载最新的release版本

`$ nvm install node`

添加npm bash completion功能

`$ npm i -g npm-completion`

查看npm配置属性

``` bash
$ npm config get globalconfig
$ npm config get userconfig
$ npm config ls -l
```

### 使用NodeSource的PPA源[安装](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions)

安装Node.js 10.x版本

``` bash
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install -y nodejs
```

## 安装Hexo

根据[Hexo官方文档](https://hexo.io/zh-cn/docs/index.html)，安装Hexo

* 安装hexo-cli，Ubuntu下全局路径在/usr/local/lib/node_modules/hexo-cli/中

`$ npm install -g hexo-cli`

* 安装hexo及package.json中指定的依赖包，安装路径在node_modules目录中

`$ npm install --save`

* 安装hexo-server（Hexo 3.0后hexo-server需要单独安装）

`$ npm install hexo-server --save`

## 本地预览

生成静态文件，路径在public目录中

`$ hexo g`

启动服务器，默认访问地址为http://localhost:4000/

`$ hexo s`

## 写博客

`$ hexo new post start-writing-hexo-blog`
