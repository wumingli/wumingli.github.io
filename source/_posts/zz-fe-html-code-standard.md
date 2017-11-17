---
layout: post
title:  "转转前端代码规范之HTML篇"
date:   2017-05-30 19:00 +0800
description: "转转前端HTML规范"
author: 武明礼
categories: front article
---

### 声明

#### 文档声明

一个DOCTYPE必须包含以下部分，并严格按照顺序出现

* 一个ASCII字符串 “<!DOCTYPE” ，大小写不敏感
* 一个或多个空白字符
* 一个ASCII字符串”html”，大小写不敏感
* 一个可选的历史遗留的DOCTYPE字符串 （DOCTYPE legacy string），或者一个可选的已过时但被允许的DOCTYPE字符串 （obsolete permitted DOCTYPE string） 字符串
* 一个或多个空白字符
* 一个编码为 U+003E 的字符 “>”

#### 团队约定
HTML文件必须加上 DOCTYPE 声明，并统一使用 HTML5 的文档声明：
`DOCTYPE用大写，html小写`

```html
<!DOCTYPE html>
```

#### 开启IE Edge模式（m端项目可忽略）
```html
<meta http-equiv="X-UA-Compatible" content="IE=Edge">
```

#### 页面必须包含 `title` 标签声明标题。

----

### 页面语言
页面语言指定为zh-CN，基于浏览器和操作系统的兼容性考虑

#### 团队约定

```html
<html lang="zh-CN">
```

### 编码
统一为`UTF-8`，且要注意字母均为`大写`，原因参考[IETF对UTF-8的定义](http://www.ietf.org/rfc/rfc3629)。

#### 团队约定

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

关于更多meta标签设定，请参考：[https://github.com/yisibl/blog/issues/1](https://github.com/yisibl/blog/issues/1)

### 代码缩进
保持一致的代码缩进，会使得IDE表现一致，定下来之后，各组要统一配置好各自的IDE。

#### 团队约定
html与js、css保持一致，用`2个空格`作为缩进单位。

### IDE
因个人喜好、电脑配置等原因，不强制统一，推荐前端神器`WebStorm(PhpStorm)`，但如上所述，代码缩进、文件编码等配置务必统一，否则多人协作同一个项目时会遇到各种坑。


### 命名
html标签名、类名、标签属性(值)和大部分属性统一用小写，class名用中划线(-)分隔。

```html
<!-- good -->
<div class="class-name"></div>

<!-- bad -->
<DIV CLASS="className"></DIV>
```
元素 `id` 必须保证页面唯一。
`id`、`class` 命名，建议单词全字母小写，单词间以 `-` 分隔。在避免冲突并描述清楚的前提下尽可能短。

#### 标签使用必须符合标签嵌套规则。

* 块元素可以包含内联元素或某些块元素，但内联元素却不能包含块元素，它只能包含其它的内联元素。例如：a元素在HTML 5规范中，是可以包含块级元素的，如果a包含任何块级元素（如p），则a自动升级为block元素。  
* p、dt只能包含内嵌元素，不能再包含块级元素的标签
* `tbody` 必须置于 `table` 中
* h1、h2、h3、h4、h5、h6包含块级元素,虽然浏览器可以解析正常，但是从语义上讲h元素本身是标题，里面应该放内嵌元素
* li 内可以包含 div 标签
* 块级元素与块级元素并列、内嵌元素与内嵌元素并列
* 不能在列表元素`<li><dt><dd>`等插入非列表兄弟元素
* 避免html节点层级太深。深层级嵌套的节点在初始化构建时往往需要更多的内存占用，并且在遍历节点时也会更慢些，这与浏览器构建DOM文档的机制有关。浏览器会把整个HTML文档的结构存储为DOM“树”结构。当文档节点的嵌套层次越深，构建的DOM树层次也会越深。


#### 不指定默认类型属性
引用css和js的标签，html5中默认已包含

```html
<!-- good -->
<link rel="stylesheet" href="" />
<script src=""></script>

<!-- bad -->
<link rel="stylesheet" type="text/css" href="" />
<script type="text/javascript" src="" ></script>
```

#### 标签闭合
* 具有开始&结束标签的，必须写起止标签
* 空元素标签，以空格+/>结尾

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

#### 标签语义化
HTML 标签的使用应该遵循标签的语义。
下面是常见标签语义：

- p - 段落
- h1,h2,h3,h4,h5,h6 - 层级标题
- strong,em - 强调
- ins - 插入
- del - 删除
- abbr - 缩写
- code - 代码标识
- cite - 引述来源作品的标题
- q - 引用
- blockquote - 一段或长篇引用
- ul - 无序列表
- ol - 有序列表
- dl,dt,dd - 定义列表

#### 标签的使用应尽量简洁，减少不必要的标签嵌套：

```html
<!-- good -->
<img class="avatar" src="image.png" alt="avatar" />
    
<!-- bad -->
<span class="avatar">
    <img class="avatar" src="image.png" alt="avatar" />
</span>

```
#### 布尔类型的属性，建议不添加属性值。
示例：

```html
<input type="text" disabled />
<input type="checkbox" value="1" checked />

```

### js、css路径引用
请以//开始，不建议写死protocol

```html
<!-- good -->
<script src="//img.58cdn.com.cn/zhuanzhuan/libs/react-lite.min.js"></script>

<!-- bad -->
<script src="https://img.58cdn.com.cn/zhuanzhuan/libs/react-lite.min.js"></script>
```
`JavaScript` 应当放在页面末尾，或采用异步加载。`script`放在页面中间将阻断页面的渲染。出于性能方面的考虑，如非必要，请遵守此条建议。

```html
<body>
   <!-- 页面元素 -->
   <script src="bundle.js"></script>
</body>
```

### 特殊字符
html源码中特殊字符务必使用字符实体，不可出现“<”和“>”等特殊字符，因为浏览器会将它们作为标签解析。

```html
<!-- good -->
<a href="#">more&gt;&gt;</a>

<!-- bad -->
<a href="#">more>></a>
```

### 表单

#### 控件标题

有文本标题的控件，尤其是checkbox/radio，必须使用 `label` 标签将其与其标题相关联。
方法如下：

* 将控件置于 `label` 内。
* `label` 的 `for` 属性指向控件的 `id`。


推荐使用第一种，减少不必要的 `id`。如果 DOM 结构不允许直接嵌套，则应使用第二种。

```html
<label>
  <input type="checkbox" name="confirm" value="on"> 我已确认上述条款
</label>

<label for="username">用户名：</label>
<input type="textbox" name="username" id="username" />
```

--

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
