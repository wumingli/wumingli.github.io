---
layout: post
title:  "组件的异步加载"
date:   2017-12-13 20:00:00 +0800
description: "聊一下异步加载React组件"
author: 武明礼
categories: 前端
tag: react
---
## 组件的异步加载，通常会有3种处理方式：

1. System.import();
2. require.ensure();
3. import();

System.import目前已废除，不多说。本文着重介绍`require.ensure`和`import`的方式来实线组件的异步加载（懒加载Lazyload）。

## 先说require.ensure

`require.ensure`配合react-router，可以轻松实现代码切割。
> react-router 2+、3+可以，4中已不支持getComponent的prop，本文用到该方法的示例中，用的是react-router@3.0.0。
> 另外，测试了一下，react/react-dom(16.2.0)与react-router 3兼容性不好，建议用正确版本搭配使用。

首先在webpack的output中配置文件名约束，否则生成的代码会是0.js/1.js，不利于调试阅读：

```js
//other config options
output: {
  //...
  chunkFilename: 'js/[name].[chunkhash:10].js',
}
//...
```

一个简单的demo：

```js
//注，require.ensure的第三个参数为上面chunkFilename中的[name]部分
const Main = (location, callback) => {
  require.ensure([], require => {
     callback(null, require('./Main/').default)
  },'main');
};
const Toast = (location, callback) => {
  require.ensure([], require => {
    callback(null, require('./Toast/').default)
  },'toast');
};
const Page1 = (location, callback) => {
  require.ensure([], require => {
    callback(null, require(`./Page1/`).default);
  }, 'page1');
};
//...
render(
  <Router history={browserHistory}>
    <Route 
      path="/"
      getComponent={Main}
    />
    <Route 
      path="/toast"
      getComponent={Toast}
    />
    <Route 
      path="/page1"
      getComponent={Page1}
    />
  </Router>,
  document.querySelector('#react')
);
```

一切正常，代码也做好了拆分：

![](https://ws2.sinaimg.cn/large/006tNc79ly1fmg95igegqj30l00b2jt8.jpg)

![](https://ws2.sinaimg.cn/large/006tNc79ly1fmg980dos7j30li0lf774.jpg)

等一下，你是前端吗，这代码写的太矬了，公用部分都没有抽出来。
（擦汗...）我改一下，提高代码的`复用性`，写个函数先：
```javascript
function requireEnsure(location, callback, name) {
  require.ensure([], require => {
    callback(null, require(`./${name}/`).default);
 }, name);
}
```

保存。呃，控制台给给出了警告：

![](https://ws2.sinaimg.cn/large/006tNc79ly1fmg3oe58gej30st0b10t3.jpg)

英文虽然不好但也能看出大意：`require函数没有使用静态提取的方式管理依赖`。
看来是忽略了一点，require、import引入模块的时候，必须要用字符串，否则webpack在编译阶段是无法识别需要引用哪个模块的。

警告而已，不管他，能实现功能就ok。试一个组件：

```js
const Page1 = (location, callback) => {
  requireEnsure(...arguments, 'Page1');
};
```

报错信息如下：

![](https://ws4.sinaimg.cn/large/006tNc79ly1fmg9f1o1v5j30g909ujsv.jpg)

“webpack不是有context吗？”嗯，那我在webpack导出模块中加入context试一下：

```javascript
module.exports = {
  //...other config
  context: process.pwd()
};
```

嗯，IDE控制台的警告消失了，预览一下页面看是不是成功了？

WTF，页面空白，打开调试工具，报错了：

![](https://ws3.sinaimg.cn/large/006tNc79ly1fmg7la4iivj30dw08rt8x.jpg)

![](https://ws4.sinaimg.cn/large/006tNc79ly1fmg7l9z53mj30dw0620t2.jpg)

感觉思路应该没问题，可能是我姿势不对，有兴趣的同学可以深挖一下，成功了别忘了同步给大家。

## 再来介绍如何用import解决异步加载组件的问题

这次先抽公共代码——虽然上面的公共代码并不能抽离:)

```js
//AsyncComponent
import React, { Component } from 'react';

const AsyncComponent = loadComponent => (
  class AsyncComp extends Component {
    state = {
      Component: null,
    };
  
    componentWillMount() {
      if (this.hasLoadedComponent()) {
        return;
      }
    
      loadComponent()
        .then(module => module.default)
        .then((Component) => {
          this.setState({Component});
        })
        .catch((err) => {
          console.error(`Cannot load component in <AsyncComp />`);
          throw err;
        });
    }
  
    hasLoadedComponent() {
      return this.state.Component !== null;
    }
  
    render() {
      const {Component} = this.state;
      return (Component) ? <Component {...this.props} /> : null;
    }
  }
);

export default AsyncComponent;
```

在使用的时候，我们只需要这样：

```js
import asyncComponent from './util/';
const Main = asyncComponent(() => import(/* webpackChunkName: "main" */ "./Main/"));
const Page1 = asyncComponent(() => import(/* webpackChunkName: "page1" */ "./Page1/"));
//other code
//...
<Router>
  <Switch>
    <Route exact path="/" component={Main} />
    <Route path="/page1" component={Page1} />
  </Switch>
</Router>
//...
```
跟require.ensuer一样，我们得到了想要的：

![](https://ws3.sinaimg.cn/large/006tNc79ly1fmh8m3ed58j30hh088ab0.jpg)

# 完了？就酱？这些我都会啊！

![](https://ws3.sinaimg.cn/large/006tNc79ly1fmh8pnzrqoj306205amx1.jpg)

那就再说个可能没怎么用过的。
在一个功能复杂、逻辑较多的项目中，仅仅通过结合Router用webpack做Code splitting远远不够，业务代码可能还是会很大。
那，其他组件怎么做异步加载？

> Talk is cheap, show me the code!

我们来个简单的栗子：

```js
//...
const Load = asyncComponent(() => import(/* webpackChunkName: "load" */ "./LoadComponent/"));
//...
<Route path="/load" component={Load} />
//LoadComponent.jsx
import React, { Component } from 'react';
import asyncComponent from '../util/';

const Page2 = asyncComponent(() => import(/* webpackChunkName: "anync-page2" */ "../Page2/"));

export default class LoadComponent extends Component {
  static defaultProps = {
    text: 'default text',
    btnText: 'click here',
    btnHandle() {
      console.log('default button handle.');
    },
  };
  constructor() {
    super();
    this.loadComponent = this.loadComponent.bind(this);
  }
  state = {
    newComp: null,
  };
  loadComponent() {
    this.setState({
      newComp: Page2,
    });
  }
  render() {
    const {
      text,
      btnText,
    } = this.props;
    return (
      <div className="toast-container">
        <span>点击下面的按钮，动态加载一个组件。</span>
        {
          this.state.newComp && <Page2 />
        }
        <p>
          <button onClick={this.loadComponent}>加载组件</button>
        </p>
      </div>
    )
  }
};
```
Page2.jsx
```js
import React, { Component } from 'react';
//顺便体验了一下16+的react中新的api，render的时候终于不用必须包一层div了
const Page2 = () => (
  [
    <h2 key={0}>Page2 content.</h2>,
    <button key={1} onClick={() => {
      console.log('clicked in page2');
    }}>page2中的点击事件</button>
  ]
);

export default Page2;
```

看一下效果：

![](https://ws3.sinaimg.cn/large/006tNc79ly1fmh92fcev4j30g808cq3u.jpg)

在业务中动态加载了组件Page2：

![](https://ws2.sinaimg.cn/large/006tNc79ly1fmh92ewqiij30jb09dabe.jpg)

Page2中的事件也能正常执行：

![](https://ws2.sinaimg.cn/large/006tNc79ly1fmh92eqh10j30j306m75a.jpg)

> 好了，就酱，谢谢你的时间。