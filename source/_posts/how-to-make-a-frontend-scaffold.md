---
layout: post
title:  "如何搭建一款前端脚手架工具"
date:   2016-11-19 10:06:00 +0800
description: "一个脚手架开发的入门级教程，要深入了解，还需要自己多动手实践"
author: 武明礼
categories: front article
---
# 前言
在我们的日常工作过程中，经常会从零开始搭建一个项目。无论是基于原生JS，还是基于jQuery/Zepto/Vue/React/Angularjs/等框架/库开发，都会有不少重复的工作要去做准备：

* 新建项目
* 拟定目录结构
* 新建html/js/css等静态文件
* 引入库文件
* 引入公共资源
* 编写项目构建配置文件

每到这时候我们就希望，如果敲一个命令，项目就自动创建，省去上面每次都要重复手动去做的事，那就太幸福了。

幸运的是，很早之前大神们就已经开始制作了一系列的脚手架来“生成”项目，如`vue-cli`/`react-native-cli`/`grunt-cli`/`gulp`等。运行一个命令，即可创建一个定制好的项目。有兴趣的同学可以阅读一下react-native-cli的源码，因为它的脚手架其实实现起来比较简单，而且源码的可读性非常好。

## npm简介

提到脚手架开发，先说一下`npm`。`npm`是啥呢，它的全称是Node Package Manager，最初是用来解决Node.js的包管理器，所以它是Node.js的“亲儿子”。后来随着其他构建工具（browserify、webpack）的发展，也随着npm的版本更新，到现在，npm基本已经一统天下了。它的新口号也变成了the package manager for Javascript。截止今日（2016/12/06），npm已经有`380,516`个包了。

安装包有两种模式，一种是本地安装，一种是全局安装。

### 本地安装

```code
npm install gulp
```

运行上面的命令，会在当前目录下生成node\_modules文件夹（如果没有），并且将gulp模块下载到这个文件夹中。

### 全局安装

直接在命令行中使用，如grunt/fis/gulp/react-native等

```code
npm install -g jshint
```

安装完毕后，就可以在命令行中使用jshint这个工具了：

```code
jshint index.js
```

可以使用下面的命令查看全局的包安装在了什么位置：

```code
npm prefix -g
```

## 使用package.json

当项目需要依赖多个包时，使用package.json才是最好的方法。package.json就是一个普通的json文件，对比手动安装的优点如下：

* 以文档形式规定了项目所依赖的包；
* 可以确定每个包所使用的版本（因为版本升级带来的坑可能不少同学都踩过，说起来都是泪啊）；
* 项目的构建可重复，在多人协作中更加方便；

package.json可以手动新建，也可以使用npm init命令填入各种信息，然后生成。

一个package.json文件必须包含两个字段：`name` `version`

```code
{
    "name": "my-project-name",
    "version": "1.0.0"
}
```

更多字段信息在此不展开解释，感兴趣的同学可以自行[查阅文档](http://blog.csdn.net/woxueliuyun/article/details/39294375)。

在此专门拿出package.json的用意主要为了说明`如何规定项目的依赖包`，在package.json文件中可以定义两种类型的包：

* dependencies: 在生产环境中需要依赖的包；
* devDependencies: 仅在开发和测试环节中需要依赖的包。

两种管理方式：

* 手动在package.json中添加这些内容；
* 使用`npm install`的--save和--save-dev命令。--save可以把它的信息自动写进package.json的dependencies字段，包括包名+版本；--save-dev写入devDependencies中

### 顺便提一下`npm scripts`：

可以在package.json使用scripts字段定义脚本命令：

```code
{
    "scripts": {
        "build": "node build.js",
        "fis3": "fis3 server stop && fis3 server clean && fis3 server start && fis3 release qa"
    }
}
```

其中，命令的值可以是任意Shell脚本。定义好脚本之后，可以运行`npm run fis3`的命令来执行。更详细的使用指南，参见阮一峰老师写的[《npm scripts使用指南》](http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html)

# !!!DUANG!!! 脚手架开发主题正式开始

这里我们以运营组的acvitivity-react-cli作为demo，以下是本demo的基础目录结构:::

![](http://ww2.sinaimg.cn/large/65e4f1e6jw1fai6egax8nj20f40bjmxy.jpg)

先看下package.json中的内容：

```json
{
  "name": "activity-react-cli",
  "version": "1.0.3",
  "description": "转转运营活动脚手架Reactjs版",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "bin": {
    "actr": "index.js"
  },
  "repository": {
    "type": "git",
    "url": "git@gitlab.58corp.com:wumingli/activity-react-cli.git"
  },
  "keywords": [
    "activity",
    "cli",
    "es6"
  ],
  "author": "Wu Mingli",
  "license": "MIT",
  "bugs": {
    "url": "http://gitlab.58corp.com/wumingli/activity-react-cli/issues"
  },
  "homepage": "http://gitlab.58corp.com/wumingli/activity-react-cli",
  "dependencies": {
    "chalk": "^1.1.3",
    "co": "^4.6.0",
    "co-prompt": "^1.0.0",
    "commander": "^2.9.0",
    "mem-fs-editor": "^2.3.0",
    "mem-fs": "^1.1.3",
    "glob": "^7.1.1",
    "semver": "^5.0.3",
    "prompt": "^0.2.14"
  }
}
```

"bin"字段是重点，脚手架开发完成后，可以用npm link（本地安装），或者npm install -g activity-react-cli（全局安装）来建立软链接，`actr`命令才能得以运行。

