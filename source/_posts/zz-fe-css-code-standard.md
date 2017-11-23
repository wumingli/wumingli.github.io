---
layout: post
title:  "转转前端代码规范之CSS篇"
date:   2017-06-01 21:12 +0800
description: "转转前端CSS规范"
author: 武明礼
categories: css
---

## 1 规范
> 目前各团队使用的css编译比较混乱，有less，有sass，有纯css，建议统一一种css编译器，可根据使用量或者投票决定具体使用哪种。

csslint的要求比较严格，且有些针对ie的规则，不适合目前移动端开发，暂不接入。

在样式文件中，需要加上@charset规则，并且一定要在样式文件的第一行首个字符位置，编码为“UTF-8”；

```css
@charset "UTF-8";
.zz-wrapper {}
```

## 2 代码风格
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
    - 左括号与类名间加1个空格；
    - 冒号与属性值之间加1个空格；
    - 逗号分隔的取值，后面加一个空格；
* 十六进制、组合属性组，能缩写的尽量缩写
* 为0的属性值后面不加单位
* 属性值需要用引号的，用双引号

示例

```css
.zz-wapper {
  width: 100px;
  box-shadow: 1px 1px 1px #333, 0 2px 0 #ccc;
  fontFamily: "Arial";
  background: url(../img/bg.png) no-repeat fixed center 100% / 100%;
}
```

### 书写顺序
css书写顺序在各团队制定规范时比较不好统一，这个也暂不做硬性要求，严格按照约定顺序书写css的话，估计项目Delay的情况会经常发生。

### css3私有前缀
构建工具中建议指定autoprefixer的版本为：
autoprefixer?browsers=last 11 iOS versions

autoprefixer的配置问题，可以通过以下网站确认需要兼容的版本 [http://browserl.ist/](http://browserl.ist/)

关于前缀、版本号、浏览器的配置，可参考[https://github.com/ai/browserslist#queries](https://github.com/ai/browserslist#queries)

类似下面的代码，类似这种，需要单冒号和双冒号的兼容情况不是很友好，若有用到需注意：

```css
/*placeholder字体颜色*/         
::-webkit-input-placeholder { /* WebKit browsers */  
  color: #ccc;  
}  
:-moz-placeholder { /* Mozilla Firefox 4 to 18 */  
  color: #ccc;  
}  
::-moz-placeholder { /* Mozilla Firefox 19+ */  
  color: #ccc;  
}  
:-ms-input-placeholder { /* Internet Explorer 10+ */  
  color: #ccc;  
}
```

### 属性选择器中的值必须用双引号包围。

`不允许`使用单引号，`不允许`不使用引号。


示例：

```css
/* good */
article[character="juliet"] {
    voice-family: "Vivien Leigh", victoria, female;
}
    
/* bad */
article[character='juliet'] {
    voice-family: "Vivien Leigh", victoria, female;
}
```


### 属性定义必须另起一行。

这样书写可以获得更准确的错误报告。

示例：

```css
/* good */
.selector {
    margin: 0;
    padding: 0;
}
    
/* bad */
.selector { margin: 0; padding: 0; }
```

### 属性定义后必须以分号结尾。

示例：

```css
/* good */
.selector {
    margin: 0;
}
    
/* bad */
.selector {
    margin: 0
}
```

## 3 通用


### 3.1 选择器   

* 尽量少用多重选择器或后代选择器，因为这会影响性能
* 选择器的嵌套层级应不大于 3 级，位置靠后的限定条件应尽可能精确。
* 如无必要，不得为 `id`、`class` 选择器添加类型选择器进行限定

```css
/* good */
#error,
.danger-message {
    font-color: #c00;
}
    
/* bad */
dialog#error,
p.danger-message {
    font-color: #c00;
}
```


* 对于通用元素（例如`div`,`p`等)使用 `class` ，这样利于渲染性能的优化
* 对于经常出现的组件，避免使用属性选择器（例如，`[class^="..."]`）浏览器的性能会受到这些因素的影响
* 选择器要尽可能短，并且尽量限制组成选择器的元素个数，建议不要超过 3 
* 只有在必要的时候才将 `class` 限制在最近的父元素内（也就是后代选择器）（例如，不使用带前缀的 `class` 时 -- 前缀类似于命名空间）

```css
/* Bad example */
span { }
.page-container #stream .stream-item .tweet .tweet-header .username { }
.avatar { }
    
/* Good example */
.avatar { }
.tweet-header .username { }
.tweet .avatar { }
```

### 3.2 属性缩写
在可以使用缩写的情况下，尽量使用属性缩写。

示例：

```css
/* good */
.post {
    font: 12px/1.5 arial, sans-serif;
}
    
/* bad */
.post {
    font-family: arial, sans-serif;
    font-size: 12px;
    line-height: 1.5;
}
```

使用 `border` / `margin` / `padding` 等缩写时，应注意隐含值对实际数值的影响，确实需要设置多个方向的值时才使用缩写。

解释：

`border` / `margin` / `padding` 等缩写会同时设置多个属性的值，容易覆盖不需要覆盖的设定。如某些方向需要继承其他声明的值，则应该分开设置。


示例：

```css
/* centering <article class="page"> horizontally and highlight featured ones */
article {
    margin: 5px;
    border: 1px solid #999;
}
    
/* good */
.page {
    margin-right: auto;
    margin-left: auto;
}
    
.featured {
    border-color: #69c;
}
    
/* bad */
.page {
    margin: 5px auto; /* introducing redundancy */
}
    
.featured {
    border: 1px solid #69c; /* introducing redundancy */
}
```

### 3.4 class命名  

class 名称中只能出现小写字符和破折号（-）
不要使用下划线，或驼峰命名法。破折号应当用于相关 `class` 的命名。

示例：  

    .btn{
      width:20px;
    } 
    
    .btn-danger{
      color:red
    }

* 避免过度任意的简写
`.btn` 代表 `button`，但是 `.s` 不能表达任何意思。

* `class` 名称应当尽可能短，并且意义明确
* 尽量不要使用缩写，除非一看就明白或特别长的单词；
* 使用有意义的名称使用有组织的或目的明确的名称，不要使用表现形式（presentational）的名称
* 基于最近的父 `class` 或基本（`base`） `class` 作为新 `class` 的前缀

```css
/* Bad example */
.t { }
.red { }
.header { }
    
/* Good example */
.tweet { }
.important { }
.tweet-header { }
```


### 3.5 注释  

* 代码是由人编写而且是写给人看的并维护的
* 请确保你的代码能够自描述、注释良好
* 对于较长的注释，务必书写完整的句子；对于一般性注解，可以书写简洁的短语


### 3.6 单行规则声明

* 对于只包含一条声明的样式，为了易读性和便于快速编辑，建议将语句放在同一行

```css
/* 只有一个声明，放在一行即可 */
.span1 { width: 60px; }
.span2 { width: 140px; }
.span3 { width: 220px; }
    
/* 多条声明，一行一个 */
.sprite {
  display: inline-block;
  width: 16px;
  height: 15px;
  background-image: url(../img/sprite.png);
}
```


### 3.7 清除浮动
当元素需要撑起高度以包含内部的浮动元素时，通过对伪类（:before/:after）设置 `clear` 或触发 `BFC` 的方式进行 `clearfix`。避免使用增加空标签的方式。

如希望使用更小副作用的清除浮动方法，参见 [A new micro clearfix hack](http://nicolasgallagher.com/micro-clearfix-hack/) 一文。

### 3.8 !important
避免使用 `!important` 声明。
当需要强制指定样式且不允许任何场景覆盖时，通过标签内联的方式（或者在某些场景下用JS指定内联）定义样式。


### 3.9 @import（这里仅指纯css文件中的import，不包括sass、less）
尽量不使用 `@import`。

与 `<link>` 标签相比，`@import` 指令要慢很多，不光增加了额外的请求次数，还会导致不可预料的问题替代办法有以下几种：

- 使用多个 `<link>` 元素
- 通过Sass 或 Less 类似的 CSS 预处理器将多个 CSS 文件编译为一个文件
- 通过webpack、gulp等构建工具上线前将多个CSS文件合并成一个


### 3.10 z-index
将 `z-index` 进行分层，对文档流外绝对定位元素的视觉层级关系进行管理。
同层的多个元素，如多个由用户输入触发的 Dialog，在该层级内使用相同的 `z-index` 或递增 `z-index`。

建议每层包含100个 `z-index` 来容纳足够的元素，如果每层元素较多，可以调整这个数值。

* z-index建议的取值

| 类型 | 范围 |
| -------- | -------- |
| 普通元素   | 0-100   |
| 吸顶/吸底/漂浮   | 101～200   |
| 弹窗   | 201～300   |
| Loading/分享蒙层   | 301～400   |
| 横屏提示   | 1000   |


### 3.11 alink的样式书写  
A标签伪类书写顺序按照a:link，a:visited，a:hover，a:active
要严格按此顺序，否则在某些浏览器中会失效。在CSS定义中，a:hover必须被置于`a:link`和`a:visited`之后，才是有效的；`a:active`必须被置于`a:hover`之后，才是有效的。

### 3.12 body背景色  
body元素最好设置`background-color` 属性，比如当body背景色使用白色时，虽然默认是白色，但还是应该显式的声明`background-color: "#fff";` 这样可以避免用户将默认值修改后引起兼容性问题。


## 4 值与单位

### 4.1 文本
文本内容必须用双引号包围。

示例：

```css
/* good */
html[lang|="zh"] q:before {
    font-family: "Microsoft YaHei", sans-serif;
    content: "“";
}
    
html[lang|="zh"] q:after {
    font-family: "Microsoft YaHei", sans-serif;
    content: "”";
}
    
/* bad */
html[lang|=zh] q:before {
    font-family: 'Microsoft YaHei', sans-serif;
    content: '“';
}
    
html[lang|=zh] q:after {
    font-family: "Microsoft YaHei", sans-serif;
    content: "”";
}
```

### 4.2 数值
当数值为 0 - 1 之间的小数时，省略整数部分的 `0`。
    
示例：

```css
/* good */
panel {
    opacity: .8;
}
    
/* bad */
panel {
    opacii: 0.8;
}
```

`url()` 函数中的路径不加引号。

示例：

```css
body {
    background: url(bg.png);
}
```

`url()` 函数中的绝对路径可省去协议名。


示例：
```css
body {
    background: url(//img.58cdn.com/img/bg.png) no-repeat 0 0;
}
```


### 4.4 长度
长度为 `0` 时须省略单位。 (也只有长度单位可省)

示例：

```css
/* good */
body {
    padding: 0 5px;
}
    
/* bad */
body {
    padding: 0px 5px;
}
```


### 4.5 颜色
RGB颜色值必须使用十六进制记号形式 `#rrggbb`。不允许使用 `rgb()`。 
带有alpha的颜色信息可以使用 `rgba()`。使用 `rgba()` 时每个逗号后必须保留一个空格。


示例：

```css
/* good */
.success {
    box-shadow: 0 0 2px rgba(0, 128, 0, .3);
    border-color: #008000;
}
    
/* bad */
.success {
    box-shadow: 0 0 2px rgba(0,128,0,.3);
    border-color: rgb(0, 128, 0);
}
```

颜色值可以缩写时，必须使用缩写形式。

示例：

```css
/* good */
.success {
    background-color: #aca;
}
    
/* bad */
.success {
    background-color: #aaccaa;
}
```

不使用命名色值(red/white等)。

```css
/* good */
.success {
    color: #90ee90;
}
    
/* bad */
.success {
    color: liggreen;
}
```

`background-position`必须同时给出水平和垂直方向的位置。

初始值为 `0% 0%`，但在只有一个方向的值时，另一个方向的值会被解析为 center。为避免理解上的困扰，应同时给出两个方向的值。[background-position属性值的定义](http://www.w3.org/TR/CSS21/colors.html#propdef-background-position)


示例：

```css
/* good */
body {
    background-position: center top; /* 50% 0% */
}
    
/* bad */
body {
    background-position: top; /* 50% 0% */
}
```

## 5 变换与动画

使用 `transition` 时应指定 `transition-property`。

示例：

```css
/* good */
.box {
    transition: color 1s, border-color 1s;
}
    
/* bad */
.box {
    transition: all 1s;
}
```

尽可能在浏览器能高效实现的属性上添加过渡和动画。
见[本文](http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/)，在可能的情况下应选择这样四种变换：

* `transform: translate(npx, npx);`
* `transform: scale(n);`
* `transform: rotate(ndeg);`
* `opacity: 0..1;`

典型的，可以使用 `translate` 来代替 `left` 作为动画属性。

示例：

```css
/* good */
.box {
    transition: transform 1s;
}
.box:hover {
    transform: translate(20px); /* move right for 20px */
}
    
/* bad */
.box {
    left: 0;
    transition: left 1s;
}
.box:hover {
    left: 20px; /* move right for 20px */
}
```

## 6 工具

* 校验css代码是否符合标准 [cssLint](http://csslint.net/)
* 使用了构建工具的项目，应适当遵守该规则，比如书写方法等。

> 补充：使用less/sass的同学，注意嵌套不要太深。html的嵌套我们也不提倡多层嵌套，尽量减少层级，所以css的继承也不应该嵌套太深，那么less/sass中书写就更应该注意没有必要的嵌套关系。

## 参考文献

1. [编码规范 by @mdo](http://codeguide.bootcss.com/)
2. [Google HTML/CSS Style Guide](http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml)
3. [baidu CSS编码规范 ](https://github.com/ecomfe/spec/blob/master/css-style-guide.md)
4. [Web前端编码规范](http://www.uml.org.cn/codeNorms/201112125.asp)
