---
layout: post
title:  "转转前端代码规范之js篇"
date:   2017-06-02 11:12 +0800
description: "转转前端js规范"
author: 武明礼
categories: javascript
---

## 1 黄金准则
 
- 希望各Team遵循同一套编码规范；
- 不让其他同事在修改自己代码时感到头大——可读性比节省文件大小更重要，压缩是打包上线做的事；
- 以下规则未尽之处以`.eslintrc`（基于airbnb，根据各Team情况修正配置）规则为准；
- 允许并欢迎提出异议，本准则为大Team制定，所以会遵循大部分同学的使用习惯。但是确定规则后，也希望各位同学都可以遵守该规则。


## 2 代码风格 

### 2.1 编码

文件编码必须采用`utf-8`编码，保存格式必须是`utf-8`无BOM格式
### 2.2 缩进

代码缩进使用`2`个空格，不要使用`tab`

> airbnb的eslint规范中建议是2个空格，各组可结合本组情况使用2个或4个空格缩进代码，但请`务必不要用tab`
> 建议：老项目中保留原来的缩进单位，新项目中遵守规范用`2个空格`。

`switch` 下的 `case` 和 `default` 必须增加一个缩进层级。

示例：

```js
// good
switch (variable) {
    
    case '1':
        // do...
        break;
    
    case '2':
        // do...
        break;
    
    default:
        // do...
    
}
    
// bad
switch (variable) {
    
case '1':
    // do...
    break;
    
case '2':
    // do...
    break;
    
default:
    // do...
    
}
```

### 2.3 换行


每个独立语句结束后必须换行。

建议：每行不得超过 `120` 个字符。

解释：

超长的不可分割的代码允许例外，比如复杂的正则表达式。长字符串不在例外之列。


运算符处换行时，运算符必须在新行的行首。

示例：

```js
// good
if (user.isAuthenticated()
    && user.isInRole('admin')
    && user.hasAuthority('add-admin')
    || user.hasAuthority('delete-admin')
) {
    // Code
}
    
var result = number1 + number2 + number3
    + number4 + number5;
    
    
// bad
if (user.isAuthenticated() &&
    user.isInRole('admin') &&
    user.hasAuthority('add-admin') ||
    user.hasAuthority('delete-admin')) {
    // Code
}
    
var result = number1 + number2 + number3 +
    number4 + number5;
```

在函数声明、函数表达式、函数调用、对象创建、数组创建、`for` 语句等场景中，不允许在 `,` 或 `;` 前换行。

示例：

```js
// good
var obj = {
    a: 1,
    b: 2,
    c: 3
};
    
foo(
    aVeryVeryLongArgument,
    anotherVeryLongArgument,
    callback
);
    
    
// bad
var obj = {
    a: 1
    , b: 2
    , c: 3
};
    
foo(
    aVeryVeryLongArgument
    , anotherVeryLongArgument
    , callback
);
```

不同行为或逻辑的语句集，使用空行隔开，更易阅读。

示例：

```js
// 仅为按逻辑换行的示例，不代表setStyle的最优实现
function setStyle(element, property, value) {
    if (element == null) {
        return;
    }
    
    element.style[property] = value;
}
```

在语句的行长度超过 `120` 时，根据逻辑条件合理缩进。

示例：

```js
// 较复杂的逻辑条件组合，将每个条件独立一行，逻辑运算符放置在行首进行分隔，或将部分逻辑按逻辑组合进行分隔。
// 建议最终将右括号 ) 与左大括号 { 放在独立一行，保证与 `if` 内语句块能容易视觉辨识。
if (user.isAuthenticated()
    && user.isInRole('admin')
    && user.hasAuthority('add-admin')
    || user.hasAuthority('delete-admin')
) {
    // Code
}
    
// 按一定长度截断字符串，并使用 + 运算符进行连接。
// 分隔字符串尽量按语义进行，如不要在一个完整的名词中间断开。
// 特别的，对于 HTML 片段的拼接，通过缩进，保持和 HTML 相同的结构。
// 用了template（jsx）的项目可忽略
var html = '' // 此处用一个空字符串，以便整个 HTML 片段都在新行严格对齐
    + '<article>'
    +     '<h1>Title here</h1>'
    +     '<p>This is a paragraph</p>'
    +     '<footer>Complete</footer>'
    + '</article>';
    
// 也可使用数组来进行拼接，相对 `+` 更容易调整缩进。
var html = [
    '<article>',
        '<h1>Title here</h1>',
        '<p>This is a paragraph</p>',
        '<footer>Complete</footer>',
    '</article>'
];
html = html.join('');
    
// 当参数过多时，将每个参数独立写在一行上，并将结束的右括号 ) 独立一行。
// 所有参数必须增加一个缩进。
foo(
    aVeryVeryLongArgument,
    anotherVeryLongArgument,
    callback
);
    
// 也可以按逻辑对参数进行组合。
// 最经典的是 baidu.format 函数，调用时将参数分为“模板”和“数据”两块
baidu.format(
    dateFormatTemplate,
    year, month, date, hour, minute, second
);
    
// 当函数调用时，如果有一个或以上参数跨越多行，应当每一个参数独立一行。
// 这通常出现在匿名函数或者对象初始化等作为参数时，如 `setTimeout` 函数等。
setTimeout(
    function () {
        alert('hello');
    },
    200
);
    
order.data.read(
    'id=' + me.model.id,
    function (data) {
        me.attchToModel(data.result);
        callback();
    },
    300
);
    
// 链式调用较长时采用缩进进行调整。
$('#items')
    .find('.selected')
    .highlight()
    .end();
    
// 三元运算符由3部分组成，因此其换行应当根据每个部分的长度不同，形成不同的情况。
var result = thisIsAVeryVeryLongCondition
    ? resultA : resultB;
    
var result = condition
    ? thisIsAVeryVeryLongResult
    : resultB;
    
// 数组和对象初始化的混用，严格按照每个对象的 `{` 和结束 `}` 在独立一行的风格书写。
var array = [
    {
        // ...
    },
    {
        // ...
    }
];
```

### 2.4 语句


不省略语句结束的分号。

在 `if / else / for / do / while` 语句中，即使只有一行，也不省略块 `{...}`。

示例：

```js
// good
if (condition) {
    callFunc();
}
    
// bad
if (condition) callFunc();
if (condition) {
    callFunc();
}
```

函数定义结束不添加分号。

示例：

```js
// good
function funcName() {
}
    
// bad
function funcName() {
};
    
// 如果是函数表达式，分号是不允许省略的。
var funcName = function () {
};
```

`IIFE` 必须在函数表达式外添加 `(`，非 `IIFE` 不得在函数表达式外添加 `(`。

解释：

IIFE = Immediately-Invoked Function Expression.

额外的 ( 能够让代码在阅读的一开始就能判断函数是否立即被调用，进而明白接下来代码的用途。而不是一直拖到底部才恍然大悟。


示例：

```js
// good
var task = (function () {
   // Code
   return result;
})();
    
var func = function () {
};
    
    
// bad
var task = function () {
    // Code
    return result;
}();
    
var func = (function () {
});
```

### 2.5 命名


`变量` 使用 `小驼峰命名法`。

示例：

```js
var loadingModules = {};
```

`常量` 使用 `全部字母大写，单词间下划线分隔` 的命名方式。

示例：

```js
var HTML_ENTITY = {};
```

`函数` 使用 `Camel命名法`。

示例：

```js
    function stringFormat(source) {
    }
```

避免命名双重否定意义的变量，

解释：

`bIsNotError`, `bIsNotFound`等都不可取



`类名` 使用 `名词/大驼峰`。

示例：

```javascript
function Engine(options) {
}
```

`函数名` 使用 `动宾短语`。

示例：

```javascript
function getStyle(element) {
}
```

建议：`boolean` 类型的变量使用 `is` 或 `has` 开头。

示例：

```javascript
var isReady = false;
var hasMoreCommands = false;
```


类的 `方法` / `属性` 使用 `Camel命名法`。

示例：

```javascript
function TextNode(value, engine) {
    this.value = value;
    this.engine = engine;
}
    
TextNode.prototype.clone = function () {
    return this;
};
```

由多个单词组成的缩写词，在命名中，根据当前命名法和出现的位置，所有字母的大小写与首字母的大小写保持一致。

示例：

```javascript
function XMLParser() {
}
    
function insertHTML(element, html) {
}
    
var httpRequest = new HTTPRequest();
```

### 2.6 注释

### 2.6.1 单行注释

必须独占一行。`//` 后跟一个空格，缩进与下一行被注释说明的代码一致。


### 2.6.2 多行注释

建议：避免使用 `/*...*/` 这样的多行注释。有多行注释内容时，使用多个单行注释。

## 3 语言特性  


### 3.1 变量

变量、函数在使用前必须先定义。（ES6项目中不必遵守此条规范）

解释：

不通过 var 定义变量将导致变量污染全局环境。


示例：

```javascript
// good
var name = 'MyName';
    
// bad
name = 'MyName';
```

原则上不建议使用全局变量，对于已有的全局变量或第三方框架引入的全局变量，需要根据检查工具的语法标识。

示例：

    /* globals jQuery */
    var element = jQuery('#element-id');

`var` 声明变量时应合并，多个var声明的变量用`,+回车`分隔：


示例：

    // good
    var hangModules = [],
        missModules = [],
        visited = {};
    // bad
    var hangModules = [];
    var missModules = [];
    var visited = {};
    

变量应提前声明，即在函数或其它形式的代码块起始位置统一声明所有变量。


示例：

```javascript
// good
function kv2List(source) {
    var list = [],
      key,
      item;
    
    for (key in source) {
        if (source.hasOwnProperty(key)) {
            item = {
                k: key,
                v: source[key]
            };
    
            list.push(item);
        }
    }
    
    return list;
}
// bad
function kv2List(source) {
    var list = [];
    
    for (var key in source) {
        if (source.hasOwnProperty(key)) {
            var item = {
                k: key,
                v: source[key]
            };
    
            list.push(item);
        }
    }
    
    return list;
}
    
```

### 3.2 条件


在 Equality Expression 中使用类型严格的 `===`。仅当判断 `null` 或 `undefined` 时，允许使用 `== null`。

解释：

使用 `===` 可以避免等于判断中隐式的类型转换。


示例：

```js
// good
if (age === 30) {
    // ......
}
    
// bad
if (age == 30) {
    // ......
}
```

尽可能使用简洁的表达式。


示例：


字符串为空
    
```js
// good
if (!name) {
    // ......
}
    
if (name === '') {
    // ......
}
```
        
字符串非空
    
```javascript
// good
if (name) {
    // ......
}
    
// bad
if (name !== '') {
    // ......
}
```
        
数组非空

```js    
// good
if (collection.length) {
    // ......
}
    
// bad
if (collection.length > 0) {
    // ......
}
```
        
布尔不成立

> 此处可根据实际情况看是否需要判断=== true/=== false
    
```js
// good
if (!notTrue) {
    // ......
}
    
// bad
if (notTrue === false) {
    // ......
}
```
    
null 或 undefined

```js    
// good
if (noValue == null) {
  // ......
}
    
// bad
if (noValue === null || typeof noValue === 'undefined') {
  // ......
}
```


对于相同变量或表达式的多值条件，用 `switch` 代替 `if`。

示例：

```javascript
// good
switch (typeof variable) {
    case 'object':
        // ......
        break;
    case 'number':
    case 'boolean':
    case 'string':
        // ......
        break;
}
    
// bad
var type = typeof variable;
if (type === 'object') {
    // ......
}
else if (type === 'number' || type === 'boolean' || type === 'string') {
    // ......
}
```


如果函数或全局中的 `else` 块后没有任何语句，删除 `else`。

示例：

```js
// good
function getName() {
    if (name) {
        return name;
    }
    
    return 'unnamed';
}
    
// bad
function getName() {
    if (name) {
        return name;
    }
    else {
        return 'unnamed';
    }
} 
```

### 3.3 循环

不要在循环体中包含函数表达式，事先将函数提取到循环体外。

解释：

循环体中的函数表达式，运行过程中会生成循环次数个函数对象。

示例：

```js
// good
function clicker() {
    // ......
}
    
for (var i = 0, len = elements.length; i < len; i++) {
    var element = elements[i];
    addListener(element, 'click', clicker);
}
    
    
// bad
for (var i = 0, len = elements.length; i < len; i++) {
    var element = elements[i];
    addListener(element, 'click', function () {});
}
```

对循环内多次使用的不变值，在循环外用变量缓存。

示例：

```javascript
// good
var width = wrap.offsetWidth + 'px';
for (var i = 0, len = elements.length; i < len; i++) {
    var element = elements[i];
    element.style.width = width;
    // ......
}
    
    
// bad
for (var i = 0, len = elements.length; i < len; i++) {
    var element = elements[i];
    element.style.width = wrap.offsetWidth + 'px';
    // ......
}
```

对有序集合进行遍历时，缓存 `length`。

解释：

虽然现代浏览器都对数组长度进行了缓存，但对于一些宿主对象和老旧浏览器的数组对象，在每次 `length` 访问时会动态计算元素个数，此时缓存 `length` 能有效提高程序性能。

示例：

```js
for (var i = 0, len = elements.length; i < len; i++) {
    var element = elements[i];
    // ......
}
```

### 3.4 类型检测

### 3.4.1 类型检测

类型检测优先使用 `typeof`。对象类型检测使用 `instanceof`。`null` 或 `undefined` 的检测使用 `== null`。

示例：

```javascript
// string
typeof variable === 'string'
    
// number
typeof variable === 'number'
    
// boolean
typeof variable === 'boolean'
    
// Function
typeof variable === 'function'
    
// Object
typeof variable === 'object'
    
// RegExp
variable instanceof RegExp
    
// Array
variable instanceof Array
    
// null
variable === null
    
// null or undefined
variable == null
    
// undefined
typeof variable === 'undefined'
```

### 3.4.2 类型转换

转换成 `string` 时，使用 `+ ''`。

示例：

```javascript
// good
num + '';
    
// bad
new String(num);
num.toString();
String(num);
```

转换成 `number` 时，通常使用 `+`或者`/1`或者Number，不强制。

`string` 转换成 `number`，要转换的字符串结尾包含非数字并期望忽略时，使用 `parseInt`。

示例：

```javascript
var width = '200px';
parseInt(width, 10);
```

使用 `parseInt` 时，必须指定进制。

示例：

```javascript
// good
parseInt(str, 10);
    
// bad
parseInt(str);
```

转换成 `boolean` 时，使用 `!!`。

示例：

```javascript
var num = 3.14;
!!num;
```

`number` 去除小数点，使用 `Math.floor` / `Math.round` / `Math.ceil`，不使用 `parseInt`。

示例：

```javascript
// good
var num = 3.14;
Math.ceil(num);
    
// bad
var num = 3.14;
parseInt(num, 10);
```

### 3.5 对象


使用对象字面量 `{}` 创建新 `Object`。

示例：

```javascript
// good
var obj = {};
    
// bad
var obj = new Object();
```

对象创建时，如果一个对象的所有 `属性` 均可以不添加引号，建议所有 `属性` 不添加引号。

示例：

```javascript
var info = {
    name: 'someone',
    age: 28
};
```

对象创建时，如果任何一个 `属性` 需要添加引号，则所有 `属性` 建议添加 `'`。

解释：

如果属性不符合 Identifier 和 NumberLiteral 的形式，就需要以 StringLiteral 的形式提供。

示例：

```javascript
// good
var info = {
    'name': 'someone',
    'age': 28,
    'more-info': '...'
};
    
// bad
var info = {
    name: 'someone',
    age: 28,
    'more-info': '...'
};
```

不允许修改和扩展任何原生对象和宿主对象的原型。

示例：

```javascript
// 以下行为绝对禁止
String.prototype.trim = function () {
};
```

属性访问时，尽量使用 `.`。

解释：

属性名符合 Identifier 的要求，就可以通过 `.` 来访问，否则就只能通过 `[expr]` 方式访问。

通常在 JavaScript 中声明的对象，属性命名是使用 Camel 命名法，用 `.` 来访问更清晰简洁。部分特殊的属性（比如来自后端的 JSON ），可能采用不寻常的命名方式，可以通过 `[expr]` 方式访问。

示例：

```javascript
info.age;
info['more-info'];
```

`for...in...` 遍历对象时, 使用 `hasOwnProperty` 过滤掉原型中的属性。（eslint中默认启用guard-for-in来约束for in遍历对象时必须有一个if语句）

示例：

```javascript
var newInfo = {};
for (var key in info) {
    if (info.hasOwnProperty(key)) {
        newInfo[key] = info[key];
    }
}
```

### 3.6 字符串

字符串开头和结束使用单引号 `'`。

解释：

1. 输入单引号不需要按住 `shift`，方便输入。
2. 实际使用中，字符串经常用来拼接 HTML。为方便 HTML 中包含双引号而不需要转义写法。

示例：

```javascript
var str = '我是一个字符串';
var html = '<div class="cls">拼接HTML可以省去双引号转义</div>';
```

使用 `数组` 或 `+` 拼接字符串。

解释：

1. 使用 `+` 拼接字符串，如果拼接的全部是 StringLiteral，压缩工具可以对其进行自动合并的优化。所以，静态字符串建议使用 `+` 拼接。
2. 在现代浏览器下，使用 `+` 拼接字符串，性能较数组的方式要高。
3. 如需要兼顾老旧浏览器，应尽量使用数组拼接字符串。

示例：

```javascript
// 使用数组拼接字符串
var str = [
    // 推荐换行开始并缩进开始第一个字符串, 对齐代码, 方便阅读.
    '<ul>',
        '<li>第一项</li>',
        '<li>第二项</li>',
    '</ul>'
].join('');
    
// 使用 `+` 拼接字符串
var str2 = '' // 建议第一个为空字符串, 第二个换行开始并缩进开始, 对齐代码, 方便阅读
    + '<ul>',
    +    '<li>第一项</li>',
    +    '<li>第二项</li>',
    + '</ul>';
```
有字符串带变量连接的，用 **``** 定义字符串。

复杂的数据到视图字符串的转换过程，选用一种模板引擎（VUE、React项目可忽略）。

解释：

使用模板引擎有如下好处：

1. 在开发过程中专注于数据，将视图生成的过程由另外一个层级维护，使程序逻辑结构更清晰。
2. 优秀的模板引擎，通过模板编译技术和高质量的编译产物，能获得比手工拼接字符串更高的性能。
3. 模板引擎能方便的对动态数据进行相应的转义，部分模板引擎默认进行HTML转义，安全性更好。

几款流行前端模板引擎对比：

- artTemplate: 体积较小，在所有环境下性能高，语法灵活。
- dot.js: 体积小，在现代浏览器下性能高，语法灵活。
- etpl: 体积较小，在所有环境下性能高，模板复用性高，语法灵活。
- handlebars: 体积大，在所有环境下性能高，扩展性高。
- hogan: 体积小，在现代浏览器下性能高。
- nunjucks: 体积较大，性能一般，模板复用性高。

### 3.7 数组


使用数组字面量 `[]` 创建新数组，除非想要创建的是指定长度的数组。

示例：

```javascript
// good
var arr = [];
    
// bad
var arr = new Array();
```

遍历数组不使用 `for in`。

解释：

数组对象可能存在数字以外的属性, 这种情况下 `for in` 不会得到正确结果。

示例：

```javascript
var arr = ['a', 'b', 'c'];
    
// 这里仅作演示, 实际中应使用 Object 类型
arr.other = 'other things';
    
// 正确的遍历方式
for (var i = 0, len = arr.length; i < len; i++) {
    console.log(i);
}
    
// 错误的遍历方式
for (var i in arr) {
    console.log(i);
}
```

不因为性能的原因自己实现数组排序功能，尽量使用数组的 `sort` 方法。

解释：

自己实现的常规排序算法，在性能上并不优于数组默认的 `sort` 方法。以下两种场景可以自己实现排序：

1. 需要稳定的排序算法，达到严格一致的排序结果。
2. 数据特点鲜明，适合使用桶排。


清空数组使用 `.length = 0`。


### 3.8 函数

#### 3.8.1 函数长度

一个函数的长度控制在 `50` 行以内。


#### 3.8.2 参数设计

一个函数的参数控制在 `6` 个以内（网上建议，此数值待商榷，个人认为函数参数不应超过3个）。

解释：

除去不定长参数以外，函数具备不同逻辑意义的参数建议控制在 `6` 个以内，过多参数会导致维护难度增大。

某些情况下，如使用 AMD Loader 的 `require` 加载多个模块时，其 `callback` 可能会存在较多参数，因此对函数参数的个数不做强制限制。


通过 `options` 参数传递非数据输入型参数。

解释：

有些函数的参数并不是作为算法的输入，而是对算法的某些分支条件判断之用，此类参数建议通过一个 `options` 参数传递。

如下函数：

```javascript
/**
 * 移除某个元素
 *
 * @param {Node} element 需要移除的元素
 * @param {boolean} removeEventListeners 是否同时将所有注册在元素上的事件移除
 */
function removeElement(element, removeEventListeners) {
    element.parent.removeChild(element);
    
    if (removeEventListeners) {
        element.clearEventListeners();
    }
}
```

可以转换为下面的签名：

```javascript
/**
 * 移除某个元素
 *
 * @param {Node} element 需要移除的元素
 * @param {Object} options 相关的逻辑配置
 * @param {boolean} options.removeEventListeners 是否同时将所有注册在元素上的事件移除
 */
function removeElement(element, options) {
    element.parent.removeChild(element);
    
    if (options.removeEventListeners) {
        element.clearEventListeners();
    }
}
```

这种模式有几个显著的优势：

- `boolean` 型的配置项具备名称，从调用的代码上更易理解其表达的逻辑意义。
- 当配置项有增长时，无需无休止地增加参数个数，不会出现 `removeElement(element, true, false, false, 3)` 这样难以理解的调用代码。
- 当部分配置参数可选时，多个参数的形式非常难处理重载逻辑，而使用一个 options 对象只需判断属性是否存在，实现得以简化。



### 3.8.3 闭包

谨慎使用闭包  

解释：  

闭包保留了一个指向它封闭作用域的指针, 所以, 在给 DOM 元素附加闭包时, 很可能会产生循环引用, 进一步导致内存泄漏. 比如下面的代码:


```javascript
function foo(element, a, b) {
  element.onclick = function() { /* uses a and b */ };
}
/*这里, 即使没有使用 element, 闭包也保留了 element, a 和 b 的引用, . 由于 element 也保留了对闭包的引用, 这就产生了循环引用, 这就不能被 GC 回收. 这种情况下, 可将代码重构为:*/
    
function foo(element, a, b) {
  element.onclick = bar(a, b);
}
    
function bar(a, b) {
  return function() { /* uses a and b */ }
}
```

在适当的时候将闭包内大对象置为 `null`。

解释：

在 JavaScript 中，无需特别的关键词就可以使用闭包，一个函数可以任意访问在其定义的作用域外的变量。需要注意的是，函数的作用域是静态的，即在定义时决定，与调用的时机和方式没有任何关系。

闭包会阻止一些变量的垃圾回收，对于较老旧的 JavaScript 引擎，可能导致外部所有变量均无法回收。


### 3.8.4 空函数


空函数不使用 `new Function()` 的形式。

示例：

```javascript
var emptyFunction = function () {};
```

#对于性能有高要求的场合，建议存在一个空函数的常量，供多处使用共享。

示例：

```javascript
var EMPTY_FUNCTION = function () {};
    
function MyClass() {
}
    
MyClass.prototype.abstractMethod = EMPTY_FUNCTION;
MyClass.prototype.hooks.before = EMPTY_FUNCTION;
MyClass.prototype.hooks.after = EMPTY_FUNCTION;
```

### 3.9 面向对象


#类的继承方案，实现时需要修正 `constructor`。

解释：

通常使用其他 library 的类继承方案都会进行 `constructor` 修正。如果是自己实现的类继承方案，需要进行 `constructor` 修正。

示例：

```javascript
/**
 * 构建类之间的继承关系
 *
 * @param {Function} subClass 子类函数
 * @param {Function} superClass 父类函数
 */
function inherits(subClass, superClass) {
    var F = new Function();
    F.prototype = superClass.prototype;
    subClass.prototype = new F();
    subClass.prototype.constructor = subClass;
}
```

属性在构造函数中声明，方法在原型中声明。

解释：

原型对象的成员被所有实例共享，能节约内存占用。所以编码时我们应该遵守这样的原则：原型对象包含程序不会修改的成员，如方法函数或配置项。

```javascript
function TextNode(value, engine) {
    this.value = value;
    this.engine = engine;
}
    
TextNode.prototype.clone = function () {
    return this;
};
```

自定义事件的 `事件名` 必须全小写。

解释：

在 JavaScript 广泛应用的浏览器环境，绝大多数 DOM 事件名称都是全小写的。为了遵循大多数 JavaScript 开发者的习惯，在设计自定义事件时，事件名也应该全小写。


设计自定义事件时，应考虑禁止默认行为。

解释：

常见禁止默认行为的方式有两种：

1. 事件监听函数中 `return false`。
2. 事件对象中包含禁止默认行为的方法，如 `preventDefault`。


### 3.10 动态特性


### 3.10.1 eval


尽量避免使用 `eval` 函数。。

解释：

`eval` 调用执行代码的作用域为本地作用域,会让程序执行的比较混乱, 当 `eval()` 里面包含用户输入的话就更加危险. 可以用其他更佳的, 更清晰, 更安全的方式写你的代码, 所以一般情况下请不要使用 `eval()`，对于`json`数据的解析请使用`JSON.parse`或库。

如果有特殊情况需要使用直接 `eval`，需在代码中用详细的注释说明为何必须使用直接 `eval`，不能使用其它动态执行代码的方式，同时需要其他资深工程师进行 Code Review。


### 3.10.2 动态执行代码

使用 `new Function` 执行动态代码。

解释：

通过 `new Function` 生成的函数作用域是全局使用域，不会影响当当前的本地作用域。如果有动态代码执行的需求，建议使用 `new Function`。

示例：

```javascript
var handler = new Function('x', 'y', 'return x + y;');
var result = handler($('#x').val(), $('#y').val());
```

### 3.10.3 with


尽量不要使用 `with`。

解释：

使用 `with` 可能会增加代码的复杂度，不利于阅读和管理；也会对性能有影响。大多数使用 `with` 的场景都能使用其他方式较好的替代。所以，尽量不要使用 `with`。


### 3.10.4 delete


减少 `delete` 的使用。

解释：

如果没有特别的需求，减少或避免使用 `delete`。`delete` 的使用会破坏部分 JavaScript 引擎的性能优化。


处理 `delete` 可能产生的异常。

解释：

对于有被遍历需求，且值 `null` 被认为具有业务逻辑意义的值的对象，移除某个属性必须使用 `delete` 操作。

在严格模式或 IE 下使用 `delete` 时，不能被删除的属性会抛出异常，因此在不确定属性是否可以删除的情况下，建议添加 `try-catch` 块。

示例：

```javascript
try {
    delete o.x;
}
catch (deleteError) {
    o.x = null;
}
```

#### 3.10.5 对象属性


避免修改外部传入的对象。

解释：

JavaScript 因其脚本语言的动态特性，当一个对象未被 seal 或 freeze 时，可以任意添加、删除、修改属性值。

但是随意地对 非自身控制的对象 进行修改，很容易造成代码在不可预知的情况下出现问题。因此，设计良好的组件、函数应该避免对外部传入的对象的修改。

下面代码的 **selectNode** 方法修改了由外部传入的 **datasource** 对象。如果 **datasource** 用在其它场合（如另一个 Tree 实例）下，会造成状态的混乱。

```javascript
function Tree(datasource) {
    this.datasource = datasource;
}
    
Tree.prototype.selectNode = function (id) {
    // 从datasource中找出节点对象
    var node = this.findNode(id);
    if (node) {
        node.selected = true;
        this.flushView();
    }
};
```

对于此类场景，需要使用额外的对象来维护，使用由自身控制，不与外部产生任何交互的 **selectedNodeIndex** 对象来维护节点的选中状态，不对 **datasource** 作任何修改。

```javascript
function Tree(datasource) {
    this.datasource = datasource;
    this.selectedNodeIndex = {};
}
    
Tree.prototype.selectNode = function (id) {
    
    // 从datasource中找出节点对象
    var node = this.findNode(id);
    
    if (node) {
        this.selectedNodeIndex[id] = true;
        this.flushView();
    }
    
};
```

除此之外，也可以通过 deepClone 等手段将自身维护的对象与外部传入的分离，保证不会相互影响。

### 3.11

上线前使用构建工具删除无用的注释、console、alert（原生alert不建议使用，警告信息应该用自定义的Dialog或Toast进行提示）

## 4 DOM  

### 4.1 元素获取

对于单个元素，尽可能使用 `document.querySelector(All)` 获取。


对于多个元素的集合，尽可能使用 `context.querySelector(All)` 获取。其中 `context` 可以为 `document` 或其他元素。`tagName` 参数为 `*` 可以获得所有子元素。

遍历元素集合时，尽量缓存集合长度。如需多次操作同一集合，则应将集合转为数组。

解释：

原生获取元素集合的结果并不直接引用 DOM 元素，而是对索引进行读取，所以 DOM 结构的改变会实时反映到结果中。

示例：

```html
<div></div>
<span></span>
    
<script>
var elements = document.getElementsByTagName('*');
    
// 显示为 DIV
alert(elements[0].tagName);
    
var div = elements[0];
var p = document.createElement('p');
docpment.body.insertBefore(p, div);
    
// 显示为 P
alert(elements[0].tagName);
</script>
```

获取元素的直接子元素时使用 `children`。避免使用`childNodes`，除非预期是需要包含文本、注释和属性类型的节点。



#### 4.2 样式获取


获取元素实际样式信息时，应使用 `getComputedStyle` 或 `currentStyle`。

解释：

通过 style 只能获得内联定义或通过 JavaScript 直接设置的样式。通过 CSS class 设置的元素样式无法直接通过 style 获取。



### 4.3 样式设置


尽可能通过为元素添加预定义的 className 来改变元素样式，避免直接操作 style 设置。

通过 style 对象设置元素样式时，对于带单位非 0 值的属性，不允许省略单位。

解释：

除了 IE，标准浏览器会忽略不规范的属性值，导致兼容性问题。


#### 4.4 DOM 操作优化


操作 `DOM` 时，尽量减少页面 `reflow`。

解释：

页面 reflow 是非常耗时的行为，非常容易导致性能瓶颈。下面一些场景会触发浏览器的reflow：

- DOM元素的添加、修改（内容）、删除。
- 应用新的样式或者修改任何影响元素布局的属性。
- Resize浏览器窗口、滚动页面。
- 读取元素的某些属性（offsetLeft、offsetTop、offsetHeight、offsetWidth、scrollTop/Left/Width/Height、clientTop/Left/Width/Height、getComputedStyle()、currentStyle(in IE)) 。


尽量减少 `DOM` 操作。

解释：

DOM 操作也是非常耗时的一种操作，减少 DOM 操作有助于提高性能。举一个简单的例子，构建一个列表。我们可以用以下三种方式：

1. 在循环体中 createElement 并 append 到父元素中。
2. 在循环体中拼接 HTML 字符串，循环结束后写父元素的 innerHTML。
3. 在循环体中通过createElement并append到documentFragment来拼接dom片段, 结束循环后把fragment append到html中

第一种方法看起来比较标准，但是每次循环都会对 DOM 进行操作，性能极低。在这里推荐使用第二种方法。



### 4.5 DOM 事件绑定

优先使用 `addEventListener / attachEvent` 绑定事件，避免直接在 HTML 属性中或 DOM 的 `expando` 属性绑定事件处理。

解释：

expando 属性绑定事件容易导致互相覆盖。


使用 `addEventListener` 时第三个参数使用 `false`。

解释：

标准浏览器中的 addEventListener 可以通过第三个参数指定两种时间触发模型：冒泡和捕获。而 IE 的 attachEvent 仅支持冒泡的事件触发。所以为了保持一致性，通常 addEventListener 的第三个参数都为 false。


在没有事件自动管理的框架支持下，应持有监听器函数的引用，在适当时候（元素释放、页面卸载等）移除添加的监听器。

尽量使用事件代理。

事件绑定的3种方式：

- 直接在DOM里绑定事件
- document.getElementById("btn").onclick = function（）{ alert("hello"); } //脚本里面绑定
- document.getElementById("btn").addEventListener("click", clickone, false); //通过侦听事件处理相应的函数

> 说明：1，2两种方式，事件相同时后一个方法会把前一个覆盖，3会多个方法同时存在并且按顺序执行，1的写法不美观

## 5 工具
- 官方网站 [http://eslint.org/](http://eslint.org/)
- 英文不是太好的同学可以阅读中文版： [http://eslint.cn/](http://eslint.cn/)

以下是运营活动脚手架中eslintrc文件中的配置，仅做参考：`
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
    "indent": ["error", 2, { "SwitchCase": 1 }],
    "no-global-assign": 0,
    "no-unsafe-negation": 0,
    "spaced-comment": 0,
    "react/react-in-jsx-scope": 0,
    "react/no-deprecated": 0,
    "react/require-extension": "off",
    "linebreak-style": ["off", "windows"]
  }
}
```

运营活动.editorconfig文件配置如下：

```code
root = true

[*]
indent_style = space
indent_size = 2
trim_trailing_whitespace = true
end_of_line = lf
insert_final_newline = true

[docs/rules/linebreak-style.md]
end_of_line = disabled

[{docs/rules/{indent.md,no-mixed-spaces-and-tabs.md}]
indent_style = disabled
indent_size = disabled

[docs/rules/no-trailing-spaces.md]
trim_trailing_whitespace = false
```

## 6 参考文献

1. [浅谈 JavaScript 编程语言的编码规范](http://www.ibm.com/developerworks/cn/web/1008_wangdd_jscodingrule/)

2. [Google JavaScript 规范](http://docs.kissyui.com/1.4/docs/html/tutorials/style-guide/google-js-style.html)

3. [baidu JavaScript编码规范 ](https://github.com/ecomfe/spec/blob/master/javascript-style-guide.md)

4. [阮一峰Javascript编程风格](http://www.ruanyifeng.com/blog/2012/04/javascript_programming_style.html)
