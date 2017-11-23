---
layout: post
title:  "脚手架创建出来的React Demo使用说明"
date:   2017-02-20 14:27:00 +0800
description: "使用Activity-react-cli创建出来的demo如何快速上手？配置，详细介绍一下本Demo架构以及使用说明"
author: 武明礼
categories: front article
---

# 如何使用脚手架创建一个本地项目

设置私服：
仅单纯的拉取私服上的依赖模块，执行下面的命令即可


如果要发布一个npm的包，需要adduser，命令如下：（过程中会提示输入用户名、密码）

> 复制的话不要复制上前面的$符号

```shell
$npm adduser --registry http://zzfe.rcnpm.58corp.com/
$npm install activity-react-cli -g --registry=http://zzfe.rcnpm.58corp.com/
#注：Mac下需要运行以下命令：
$sudo npm install activity-react-cli -g --registry=http://zzfe.rcnpm.58corp.com/
```

运行以下命令，全局安装脚手架（亦可将该包安装在任意目录，然后用npm link命令进行关联，推荐全局安装）

```code
npm install -g activity-react-cli
```

安装完后后，即可用命令`actr init [project-name] [--branch-branchName]`来创建一个运营活动，默认基于react-redux分支进行创建，并自动添加activit-前缀，自动替换demo中的项目名称（demo中名为new-webpack-project）。

> 脚手架安装完后，为避免以后安装npm包时必须经过该私服检查从而影响安装项目依赖包，可手动删除该配置项。具体步骤如下：

* npm config edit
* 找到下面三行代码并删除：

```code
unsafe-perm=true
registry=http://zzfe.58corp.com/npm
//zzfe.58corp.com/:_authToken="CyaK533ZVOHKT1nScobk2yNuylpWTCbrrqh5iSVORNU="
```

### 开发&测试&上线命令
```json
//package.json脚本定义
"scripts": {
  "dev": "NODE_ENV=dev webpack-dev-server --config webpack.config.js --inline --progress --colors",
  "test": "NODE_ENV=test webpack --config webpack.config.js --progress --colors",
  "prod": "NODE_ENV=production webpack --config webpack.config.js --progress --colors"
}
//...
```

* npm run dev ——开发环境
* npm run test ——部署至测试环境
* npm run prod ——上线，并上传至线上Jekins的FTP服务器
* http://build.58corp.com/view/others/view/zhuanzhuan/job/zhuanzhuan-fe-yunying-release-onlineenv/294/rebuild/parameterized 替换自己的项目名称，上线（下图）

![](https://ww3.sinaimg.cn/large/006tNbRwly1fcwytvj4g6j30fg03gt90.jpg)

### 合并后的webpack.config.js

拆分or合并？这里我选择了合并，利用process.env.NODE_ENV进行区分开发环境，并选择不同的output/entry/plugin进行配置，个人认为更方便管理，如果你喜欢拆分，可以自行对webpack.config.js进行拆分。

```javascript
const PROJECT_NAME = 'new-webpack-project';
//用脚手架构建出来的项目会被替换成新建的项目名，如const PROJECT_NAME = 'acitivity-reduce-auction';
```

为给项目瘦身，demo中采用了react-lite，因此，需要在配置文件中作如下映射：

```javascript
externals: {
  'react': 'React',
  'react-dom': 'React'
}
...
resolve: {
  ....
  alias: {
    react: '//img.58cdn.com.cn/zhuanzhuan/libs/react-lite.min.js'
  }
}
```

```javascript
//开发环境会自动打开系统默认浏览器，并打开该项目
const opn = require('opn');
opn(`http://127.0.0.1:${APP_PORT}`);
```

详细的可以看webpack.config.js，有任何意见或者建议都可以提出，该配置文件也会逐步完善、优化。

### 模板文件template.html

默认引入了微信分享的js库，以及react-lite库，渲染节点为id为react的div。

Loading抽离成了组件，也可以把Loading相关的HTML放到渲染节点中，以防项目入口js加载&执行前界面会白屏。

默认加入了手机端视图meta，以及屏蔽电话号码、邮件的meta标签，如不需要可自行删除。

### 项目启动（入口）文件 src/app.jsx

定义路由，以及项目的Render。如果不需要路由功能，只保留Main组件，并修改render方法第一个参数为<Main />即可。

### 主组件src/components/Main/Main.jsx

demo中的组件示例基本全在该文件中。列举几个有代表性的例子：

* ajax组件：简单的ajax抽离，项目中尽量避免用jQuery/zepto，该ajax组件可满足基本的异步操作。如果只是获取dom（当然React项目中获取dom的情形比较少）更不应该引入这些库，会使项目大很多。应该采用更优雅的获取dom的方式，如React中的refs，js中的querySelector等。
* createItem组件：采用了flux架构，具体见组件代码。
* Download组件：作了限制，如果是手机端，动态插入callapp.min.js，并显示在页面底部；PC端不显示该组件。
* List组件：加入了Lazyload功能

