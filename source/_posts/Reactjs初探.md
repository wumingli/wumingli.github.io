---
layout: post
title:  "小试牛刀——基于Reactjs的一个小Demo分享"
date:   2016-07-26 17:30:00 +0800
description: "只简单介绍Reactjs的一些基本用法，如何运用在项目中还需要深入了解它的工作原理"
author: 武明礼
categories: front article
---

# React是什么
React 是一个Facebook和Instagram用来创建用户界面的 JavaScript 库。
官方文档：[https://facebook.github.io/react/docs/getting-started.html](https://facebook.github.io/react/docs/getting-started.html)
中文翻译：[http://reactjs.cn/react/docs/getting-started.html](http://reactjs.cn/react/docs/getting-started.html)，有些翻译并不十分准确而且可能并未与官网同步更新，英文好的同学可以直接阅读官方文档。

React是`MVC`中的V

React用虚拟DOM实现了一个非常强大的渲染系统，在React中，对DOM只更新不读取。

Facebook创造 React 是为了解决一个问题：构建**随着时间推移，数据不断变化的大规模应用程序**。为了达到这个目标，React 采用下面两个主要的思想：


* `简单` 只要表达出你的应用程序在任一个时间点应该长的样子，然后当底层的数据变了，React 会自动处理所有用户界面的更新。
* `声明式（Declarative）` 数据变化后，React 概念上与点击“刷新”按钮类似，但仅会更新变化的部分。

# React用来干什么：构建可组合的组件

React专注于构建可复用的组件。事实上，通过 React 你唯一要做的事情就是构建组件。得益于React良好的封装性，组件使代码复用、测试和关注点分离更加简单。

组件可能会长下面这个样子[更详细的组件见Demo](https://github.com/wumingli/reactjs-demo){:target="_blank"}：


```javascript
var User = React.createClass({
    render() {
        let data = this.props.data;
        console.log(data);
        return(<div className="user-info">
            <dl>
                <dt><img src={data.avatar} /></dt>
                <dd>{data.name} {data.province}</dd>
                <dd>评价<i>{data.comments}</i> 买过<i>{data.buy_count}</i> 加入转转{data.join_time}</dd>
            </dl>
        </div>);
    }
});
```

你可能注意到了，return 后面那坨html标签是什么鬼？下面该JSX出场了。

# 什么是JSX

JSX 即 `Javascript XML`——一种在React组件内部构建标签的类XML语法。

当然如果你有强迫症，不喜欢这样的混合写法，也可以不使用JSX。但使用JSX可以提高组件的可读性。

不使用JSX的代码长这样：

```javascript
React.createElement(
    'a',
    {
        href: 'http://facebook.github.io/react/'
    },
    'Hello React!'
);
```

使用JSX瘦成了这样：

```html
<a href="http://facebook.github.io/react/">Hello React!</a>
```

每个人都有自己的工作流，所以JSX 并不强制必须使用的——你可以喜欢胖子。。

-------

# 组件生命周期

## 实例化

### 首次实例化

getDefaultProps

getInitialState

componentWillMount

render

componentDidMount

### 实例化完成后的更新

getInitialState

componentWillMount

render

componentDidMount

## 存在期

### 组件已存在时的状态改变

componentWillReceiveProps

shouldComponentUpdate

componentWillUpdate

render

componentDidUpdate

## 销毁&清理期

componentWillUnmount

# 说明
生命周期共提供了10个不同的API。

1.getDefaultProps

作用于组件类，只调用一次，返回对象用于设置默认的props，对于引用值，会在实例中共享。

2.getInitialState

作用于组件的实例，在实例创建时调用一次，用于初始化每个实例的state，此时可以访问this.props。

3.componentWillMount

在完成首次渲染之前调用，此时仍可以修改组件的state。

4.render

必选的方法，创建虚拟DOM，该方法具有特殊的规则：

* 只能通过this.props和this.state访问数据
* 可以返回null、false或任何React组件
* 只能出现一个顶级组件（不能返回数组）
* 不能改变组件的状态
* 不能修改DOM的输出

5.componentDidMount

真实的DOM被渲染出来后调用，在该方法中可通过this.getDOMNode()访问到真实的DOM元素。此时已可以使用其他类库来操作这个DOM。

在服务端中，该方法不会被调用。

6.componentWillReceiveProps

组件接收到新的props时调用，并将其作为参数nextProps使用，此时可以更改组件props及state。

```javascript
    componentWillReceiveProps(nextProps) {
        if (nextProps.bool) {
            this.setState({
                bool: true
            });
        }
    }
```

7.shouldComponentUpdate

组件是否应当渲染新的props或state，返回false表示跳过后续的生命周期方法，通常不需要使用以避免出现bug。在出现应用的瓶颈时，可通过该方法进行适当的优化。

在首次渲染期间或者调用了forceUpdate方法后，该方法不会被调用

8.componentWillUpdate

接收到新的props或者state后，进行渲染之前调用，此时不允许更新props或state。

9.componentDidUpdate

完成渲染新的props或者state后调用，此时可以访问到新的DOM元素。

10.componentWillUnmount

组件被移除之前被调用，可以用于做一些清理工作，在componentDidMount方法中添加的所有任务都需要在该方法中撤销，比如创建的定时器或添加的事件监听器。

# props和state

## props

props是properties的缩写，可以用它把任意数据类型的数据传递给组件。

* 可以在挂载组件的时候设置它的props：

```html
<Loading show={true} />
<!--在JSX中，可以把props设置为字符串：-->
<a href="/surveys/add">Add survey</a>
<!--也可以使用{}语法来注入JS传递任意类型的变量：-->
<a href={'/surveys/' + survey.id}>{survey.title}</a>
```

* 还可以使用展开语法把props设置成一个对象：

```javascript
var ListSurveys = React.createClass({
    render() {
        var props = {
            one: 'foo',
            two: 'bar'
        };
        return <SurveyTable {...props} />;
    }
});
```

* 或者通过组件实例的setProps方法来设置其props（很少这样做）：

* props还可以用来添加事件处理器

```html
<button onClick={this.handleClick}>test click</button>
```

## State

每一个React组件都可以拥有自己的state，state与props的区别在于前者只存在于组件内部。

state可以用来确定一个元素的视图状态，具体可见demo中的**`Loading组件`**。

state可以通过setState来修改，也可以使用getInitialState方法提供一组默认值。
只要setState被调用，render方法就会被调用。

如果render函数的返回值有变化，虚拟DOM就会更新，真实DOM也会被更新，最终用户就会在浏览器中看到变化。

注：**千万不要直接修改this.state，永远记得要通过this.setState方法修改。**

## 放在state和props的各是哪些部分

不要在state中保存计算出的值，理想情况下应该只保存最简单的数据，即那些组件正常工作时的必要数据。比如一个复选框的勾选状态、demo中的显示Loading开关。

不要尝试把props复制到state中，尽可能把props当作数据源。

# 事件处理

React通过将事件处理器绑定到组件上来处理事件，在事件被触发的同时来更新组件内部的状态，从而引发组件重绘。

视图层想要渲染出事件触发后的结果，只需要在渲染函数中判断组件的内部状态（如this.state）。

```html
<button onClick={this.handleClick}>查看更多留言</button>
```

**说明** 上面代码写法上类似平时不推荐的内联事件处理器属性，但实际上React在底层实现上并没有使用HTML的onclick，只是用这种写法来绑定事件处理器，其内部则按照需要更高效地维护着事件处理器。

React对处理各种事件类型提供了友好的支持，具体见其[事件系统](https://facebook.github.io/react/docs/events.html){:target="_black"}

其中绝大部分事件不需要额外的处理就能正常使用，但触控事件需要通过调用以下代码启用：

```javascript
React.initializeTouchEvents(true);
```

## 根据状态进行渲染

上面说过，setState会引起render被调用，所以可以在render函数中通过this.state渲染不同的状态。

```javascript
render() {
    let loadingStatus = '';
    if (this.state.show) {
        loadingStatus = ' on';
    }
    ...
    return (
        ...
        <div className={'loading' + loadingStatus} ...>
        ...
    );
}
```
再次提醒，永远不要尝试通过setState或replaceState以外的方式去修改state对象，如this.state.show = true就不是个好做法，因为它无法通知React是否需要重新渲染组件，而且可能会导致下次调用setState时出现意外结果。

# 复合组件

React组件中不存在继承，而是通过组件之间的组合（复合）来构建应用。所以用“父、子组件”来表达可能不太合适，因为它们的关系是复合，而非继承。表面上看，某个组件调用了另外一个组件，像是父子关系，所以我们这里还是叫它们父组件或子组件。

本质上，React组件就是一个Javascript函数，它接受属性（props）和状态（state）作为参数，并输出渲染好的HTML。

复合组件的例子可以参照Demo中的detail组件。

## 组件间的通信

ReactJS中数据的流动是单向的，父组件的数据可以通过设置子组件的props传递数据给子组件。

如果想让子组件改变父组件的数据，可以在父组件中传一个callback(回调函数)给子组件，子组件内调用这个callback即可改变父组件的数据。

更详细的组件之间的通信，可以参考[这篇文章](http://www.alloyteam.com/2016/01/some-methods-of-reactjs-communication-between-components/){:target="_black"}。

# 动画

CssTranstionGroup：React组件进入或者离开DOM的时候，简单地执行CSS过渡和动画。
requestAnimationFrame：利用componentWillUpdate触发。

# Refs

在从 render 方法中返回UI结构之后，突破React的虚拟DOM限制，返回真实的DOM元素。

用法如下：

* 绑定一个 ref 属性到 render 返回的东西上面去，例如：

```html
<input ref="myInput" />
```

* 在其它代码中（典型地事件处理代码），通过 this.refs 获取*支撑实例（ backing instance ）*：

```javascript
this.refs.myInput
```

这样就可以通过调用 this.refs.myInput.getDOMNode() 直接获取到组件的真实DOM节点。

# 路由

路由是在单页面应用（SPA）中为URL指定处理器函数。

React仅仅是一个渲染类库，自身不具备路由功能。不过有很多路由模块可以搭配React使用。因为分享的是React，所以Demo中采用的是react-router，是由React Component构成的。

常用的与React搭配的路由模块：

* Backbone.Router
* Aviator
* react0-router
* Om(ClojureScript)

# 关于公共组件的开发

崇跃问的一个问题，组件样式的抽离是否可以实现，试了一下是可以的。开发时把组件样式放到单独的css（less/sass）中，在组件内require即可。具体见popup或animate组件。

# webpack配置

在package.json中指定dev和release引用的配置文件，demo中指定了webpack.config.pro.js用于完成后的打包工作。
