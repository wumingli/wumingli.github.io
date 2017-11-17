---
layout: post
title:  "转转前端代码规范"
date:   2017-05-17 14:06 +0800
description: "本规范基于W3C规范，结合&整合各团队业务制定，一旦确定请严格遵守"
author: 武明礼
categories: html
---
<!--# 概述

本`前端代码规范`由转转各组前端整理而成，目的是统一编程风格，提高代码质量，加强编码规范。
-->

# HTML规范
因目前大家手中多为SPA项目，已实现前后端分离，所以日常html规范可能用的比较少，但页面制作作为前端工程师工作中的一部分，仍旧需要遵守一些规范约束。整理如下：

### 文档声明

```html
<!DOCTYPE html>
```

### 页面语言
页面语言指定为zh-CN，基于浏览器和操作系统的兼容性考虑

```html
<html lang="zh-CN">
```

### 编码
统一为`UTF-8`，且要注意字母均为`大写`，原因参考[IETF对UTF-8的定义](http://www.ietf.org/rfc/rfc3629)。

```html
<meta charset="UTF-8" />
```

### DNS预解析
```html
<link rel="dns-prefetch" href="https://img.58cdn.com.cn">
```

### 手机端页面meta设定
因大多数页面处于端内或微信webview中，诸如name为apple-mobile-web-app-capable、apple-mobile-web-app-status-bar-style等meta标签可根据实际页面适配的端自行添加，但viewport、format-detection的设定应保持一致：

```html
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" />
<meta name="format-detection" content="telephone=no, email=no">
```

### 代码缩进（2 or 4 ？待商榷）
保持一致的代码缩进，会使得IDE表现一致，定下来之后，各组要统一配置好各自的IDE。

### IDE
因个人喜好、电脑配置等原因，不强制统一，推荐前端神器`WebStorm(PhpStorm)`，但如上所述，代码缩进、文件编码等配置务必统一，否则多人协作同一个项目时会遇到各种坑。


### 书写风格
html标签名、类名、标签属性(值)和大部分属性统一用小写，class名用中划线(-)分隔。

```html
<!-- good -->
<div class="class-name"></div>

<!-- bad -->
<DIV CLASS="className"></DIV>
```

### 不指定默认类型属性
引用css和js的标签，html5中默认已包含

```html
<!-- good -->
<link rel="stylesheet" href="" >
<script src=""></script>

<!-- bad -->
<link rel="stylesheet" type="text/css" href="" >
<script type="text/javascript" src="" ></script>
```

### 标签闭合
* 具有开始&结束标签的，必须写起止标签
* 空元素标签，以空格+>结尾

此闭合规范与Reactjs中Eslint要求恰好保持一致，VUE中模板语法可视具体情况而定

示例：

```html
<!-- good -->
<div>
    <h1>我是h1标题</h1>
    <p>我是一段文字，有始有终，浏览器能正确解析</p>
</div>
    
<br />
<input type="button" value="button" />

<!-- bad -->
<div>
    <h1>我是h1标题</h1>
    <p>我是一段文字，有始无终，浏览器也能正确解析
</div>
    
<br>
<input type="button" value="button" />
```

### js、css路径引用
请以//开始，不建议写死protocol

```html
<!-- good -->
<script src="//img.58cdn.com.cn/zhuanzhuan/libs/react-lite.min.js"></script>

<!-- bad -->
<script src="https://img.58cdn.com.cn/zhuanzhuan/libs/react-lite.min.js"></script>
```

### 特殊字符
html源码中特殊字符务必使用字符实体，不可出现“<”和“>”等特殊字符，因为浏览器会将它们作为标签解析。

```html
<!-- good -->
<a href="#">more&gt;&gt;</a>

<!-- bad -->
<a href="#">more>></a>
```

### 手机端模板示例
如果需要考虑SEO，请自行添加keywords/description的meta标签(PC模板一般为必填)

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
  <meta charset="UTF-8">
  <link rel="dns-prefetch" href="https://img.58cdn.com.cn">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, shrink-to-fit=no" >
  <meta name="format-detection" content="telephone=no" >
  <title>移动端HTML模版</title>
  </head>
  <body>
    <div>
      <span>页面内容</span>
    </div>
    <script src="//img.58cdn.com/path/to/your/js"></script>
  </body>
</html>
```

### 图片格式
gif、png8、png24、jp(e)g、webp，根据图片格式特性和场景选择合适的图片格式。

原则：保证不严重失真的前提下，体积越小越好。

建议：

* 内容图以jpg(webp)为主，背景图优先考虑使用png格式；
* 小图标除制作成精灵图外，建议用font、base64；
* 半透明效果的，为避免毛边等瑕疵，用png24；
* 条件允许的情况下，webp或许是最优选择；

另：上线前`tinypng统一压缩`是个不错的习惯，当然还有其他工具；

<div style="border-top: 3px dotted #666; margin: 40px auto 10px;text-align: center;">
<span style="background-color: #fff;padding: 0 10px; position: relative; top: -14px;">我是html和css的分隔线</span>
</div>

# CSS规范
目前各团队使用的css编译比较混乱，有less，有sass，有纯css，建议统一一种css编译器，可根据使用量或者投票决定具体使用哪种。

csslint的要求比较严格，且有些针对ie的规则，不适合目前移动端开发，暂不接入。

在样式文件中，需要加上@charset规则，并且一定要在样式文件的第一行首个字符位置，编码为“UTF-8”；

```css
@charset "UTF-8";
.zz-wrapper {}
```

### 代码风格
样式书写一般分为紧凑格式（Compact）和展开格式（Expanded）。

```css
/*紧凑*/
.zz { display: inline-block; width: 100%; }
/*展开*/
.zz {
  display: inline-block;
  width: 100%;
}
```

团队约定：采用展开格式，以2个空格建为缩进单位（与后面的js保持一致），请配置好自己的IDE。

* 分号：每个属性声明末尾加分号
* 易读性：
	* 左括号与类名间加1个空格；
	* 冒号与属性值之间加1个空格；
	* 逗号分隔的取值，后面加一个空格；
* 十六进制、组合属性组，能缩写的尽量缩写
* 为0的属性值后面不加单位
* 属性值需要用引号的，用单引号

#### 示例

```css
.zz-wapper {
  width: 100px;
  box-shadow: 1px 1px 1px #333, 0 2px 0 #ccc;
  background: url('../img/bg.png') no-repeat fixed center 100% / 100%;
}
```

#### 书写顺序
css书写顺序在各团队制定规范时比较不好统一，这个也暂不做硬性要求，严格按照约定顺序书写css的话，估计项目Delay的情况会经常发生，这个锅我们不想背-_-|||

### css3私有前缀
目前的构建工具（以运营脚手架中webpack.config为例）均已设置autoprefixer
?browsers=last 7 iOS versions，目前线上运行项目中，只在flex中发现某些低端安卓机表现不佳，其余手机兼容性尚可。`请提宝贵建议`。

<div style="border-top: 3px dotted #666; margin: 40px auto 10px;text-align: center;">
<span style="background-color: #fff;padding: 0 10px; position: relative; top: -14px;">我是css和js的分隔线</span>
</div>

# javascript代码规范
JS代码规范在开启Eslint的前提条件下反而简单了：如果代码不符合规范，如果开启了preloader，开发过程中会及时报错。

比如变量用var定义时，会提示用let或者const声明，而如果改成了let，后面没有再重新对变量进行赋值等操作时，会提示用const声明该变量。

有了es6的块级作用域之后，先前在JSlit（JSHint）时代提倡的代码预先声明（一组变量声明——用逗号分隔）的规则不再适用。

如：

```javascript
var name = 'wumingli',
    sex = 'male',
    age = 32;
//code here
```

所以即使开启了Eslint，我们在Coding时还是有很大的自由度，变量用到时随时随地声明（赋值）

```javascript
const isZzApp = /zhuanzhuan/i.test(navigator.userAgent);
if (isZzApp) {
	//
} else {
	let redirectUrl = '//m.zhuanzhuan.58.com/';
	if (document.cookie.indeOf('unionid') > -1) {
		redirectUrl = '//wechat.zhuanzhuan.58.com';
	}
	location.replace(redireactUrl);
}
```

但是在一些框架语言中，变量（函数）的命名依然会有很严格的约束，比如在React中，组件变量的命名必须以大写字母开始，否则react不会认为它是一个组件，而是一个普通js变量。

```javascript
import { render } from ReactDom;
import Main from './Main';
import Trailer from './Trailer';

const isMain = new Date().getTime() > new Date('2017-05-22 23:59:59').getTime();

let RenderComponent = Trailer;
if (isMain) {
	RenderComponent = Main;
}

render(
	<RenderComponent />,
	document.querySelector('#react');
);
```

所以在命名上，我们还是要有一定的规范。

* 构造函数首字母大写
* 函数用具备该函数功能的单词命名，切忌用Func1，Func2等为函数命名。变量同理；

运营活动脚手架中，Eslint的配置文件（供参考）：

```json
{
    "extends": "airbnb",
    "parser": "babel-eslint",
    "globals": {
        "window": true,
        "React": true,
        "ReactDOM": true,
        "document": true
    },
    "plugins": [
        "react"
    ],
    "rules": {
        "arrow-parens": 0,
        "indent": [
            "error",
            2,
            {
                "SwitchCase": 1
            }
        ],
        "no-global-assign": 0,
        "no-unsafe-negation": 0,
        "no-console": 0,
        "spaced-comment": 0,
        "react/react-in-jsx-scope": 0,
        "react/no-deprecated": 0,
        "react/require-extension": "off",
        "linebreak-style": [
            "off",
            "windows"
        ]
    }
}
```

具体Airbnb的对javascript语言规范约束，见 [https://github.com/airbnb/javascript](https://github.com/airbnb/javascript)

运营活动脚手架中，babel配置文件（.babelrc）如下：

```json
{
    "presets": [
        "react",
        "es2015",
        "stage-2"
    ]
}
```

## 写在最后的话：
第一次与各小组成员讨论规范问题时，学霸抛出一个问题：制定此规范的意义是什么？成本、收益是什么？

会后仔细思考了一下这个问题，觉得有以下几点：

* 此规范为`团队规范`，大家进行项目开发，尤其是长期迭代的项目，为方便他人接手项目时不至于在代码阅读上浪费太多时间，至少我们也要保持相同的代码缩进等规则。
* 国有国法，家有家规，没有规矩不成方圆，既然大家在同一个团队里面做项目，就需要遵守相同的规则。
* 养成良好的编程习惯，是件受益终身的事情，信老司机保平安！


