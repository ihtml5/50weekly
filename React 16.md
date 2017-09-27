### **[原文](https://facebook.github.io/react/blog/2017/09/26/react-v16.0.html)**

我们很激动宣布React 16的发布!  其中提供了一些之前多次提出但尚未提供的功能，包括增加[**fragments**](#render支持返回fragments和字符串), [**错误界限**](#更好的错误处理), [**portals**](#portals), 对[**custom DOM attributes**](#支持自定义属性)的支持, 优化[**服务端渲染**](#更好的服务端渲染), 减小[**文件体积**](#减小文件体积).

### render支持返回fragments和字符串

你现在可以从一个render方法中返回一个元素数组。 像之前使用数组，你需要为每一个元素增加一个key来避免key警告。

```js
render() {
  // No need to wrap list items in an extra element!
  return [
    // Don't forget the keys :)
    <li key="A">First item</li>,
    <li key="B">Second item</li>,
    <li key="C">Third item</li>,
  ];
}
```

将来，我们可能会为jsx增加一个不需要声明keys的特殊片段语法

我们也已经支持返回字符串：

```js
render() {
  return 'Look ma, no spans!';
}
```
[看所有支持的返回类型列表](http://facebook.github.io/react/docs/react-component.html/#render)

### 更好的错误处理

以前，渲染过程中的运行时错误可能会使React处于断开的状态，从而产生隐秘的错误消息, 需要刷新网页才能恢复。

为了解决这个问题，react 16使用了一个更灵活的策略。默认，如果一个应用内部的组件的render方法或者生命周期方法出现错误，并不会让整个应用卸载，这阻止脏数据的展示。

与每次有错误时不卸载整个应用相反， 你可以使用错误边界。错误边界是捕获子树内部错误的特殊组件，并显示一个备用UI。 想到错误边界，如try-catch语句，但是对于React组件。

了解更详细的细节， 可以看之前发表的[react 16中的异常处理](http://facebook.github.io/react/blog/2017/07/26/error-handling-in-react-16.html)


### Portals

Portals 提供了一级类的方式来将子元素呈现到存在于父组件的DOM层次结构之外的DOM节点。

```js
render() {
  // React does *not* create a new div. It renders the children into `domNode`.
  // `domNode` is any valid DOM node, regardless of its location in the DOM.
  return ReactDOM.createPortal(
    this.props.children,
    domNode,
  );
}
```

看portals完整例子[portals文档](http://facebook.github.io/react/docs/portals.html).

### 更好的服务端渲染

React 16重写了服务端渲染。 现在它是相当快。 它支持**流**, 因此你可以更快地发送字节到客户端。并且多亏了[新打包策略](#减少文件体积)在编译阶段检查`process.env`(无论你相信与否， 在node里面读`process.env`是相当慢的!), 你不再需要打包react来获得更好的服务端渲染性能。

核心团队成员Sasha Aickin 写了一篇[描述react16在服务端渲染方面提升](https://medium.com/@aickin/whats-new-with-server-side-rendering-in-react-16-9b0d78585d67)的文章. 按照Sasha's synthetic benchmarks, React 16中的服务器渲染大概比React 15快三倍。“当与React 15与”process.env“进行比较编译时，node4的性能提高了2.4倍，node的性能提升了3倍 6，并且在新的Node 8.4版本中全面的改善了3.8倍，而如果您在React 15中进行比较而不进行编译，则在最新版本的Node！中，React 16在SSR中有一个完整的数量级增益。 （正如Sasha Aickin指出的那样，请注意，这些数字是基于综合基准，可能不反映现实世界的表现。

此外，React 16更好地在服务器呈现的HTML一旦到达客户端时进行注水。 它不再需要初始渲染与服务器的结果完全匹配。 相反，它将尝试尽可能重用现有的DOM。 没有更多的校验和！ 一般来说，我们不建议您在客户端和服务器上呈现不同的内容，但在某些情况下（例如时间戳）可能会有用。

看[`ReactDOMServer`文档](http://facebook.github.io/react/docs/react-dom-server.html)了解更多细节。

### 支持自定义属性

与忽略不能识别的html和svg属性相反，React现在将[传递他们给dom](https://facebook.github.io/react/blog/2017/09/08/dom-attributes-in-react-16.html).这有额外的好处，允许我们摆脱React的大多数属性白名单，从而减少文件大小。

### 减小文件体积

尽管增加了一些新功能，然而事实上react 16确是比15.6.1更小!

* `react` is 5.3 kb (2.2 kb gzipped), 之前 20.7 kb (6.9 kb gzipped).
* `react-dom` is 103.7 kb (32.6 kb gzipped), 之前 141 kb (42.9 kb gzipped).
* `react` + `react-dom` is 109 kb (34.8 kb gzipped), 之前 161.7 kb (49.8 kb gzipped).

整体上比之前版本**压缩体积减少了32%,gizp体积减少了30%**

体积差异部分归因于包的变化。 React现在使用[Rollup](https://rollupjs.org/)为每种不同的目标格式创建扁平的包，从而实现更小的体积和更好的运行时性能。 扁平打包还意味着，无论您使用Webpack，Browserify，还是UMD包还是任何其他系统，React打包后的体积都是一致的。


### MIT licensed

[In case you missed it](https://code.facebook.com/posts/300798627056246/relicensing-react-jest-flow-and-immutable-js/), React 16 is available under the MIT license. We've also published React 15.6.2 under MIT, for those who are unable to upgrade immediately.

### New core architecture

React 16 is the first version of React built on top of a new core architecture, codenamed "Fiber." You can read all about this project over on [Facebook's engineering blog](https://code.facebook.com/posts/1716776591680069/react-16-a-look-inside-an-api-compatible-rewrite-of-our-frontend-ui-library/). (Spoiler: we rewrote React!)

Fiber is responsible for most of the new features in React 16, like error boundaries and fragments. Over the next few releases, you can expect more new features as we begin to unlock the full potential of React.

Perhaps the most exciting area we're working on is **async rendering**—a strategy for cooperatively scheduling rendering work by periodically yielding execution to the browser. The upshot is that, with async rendering, apps are more responsive because React avoids blocking the main thread.

This demo provides an early peek at the types of problems async rendering can solve:

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Ever wonder what &quot;async rendering&quot; means? Here&#39;s a demo of how to coordinate an async React tree with non-React work <a href="https://t.co/3snoahB3uV">https://t.co/3snoahB3uV</a> <a href="https://t.co/egQ988gBjR">pic.twitter.com/egQ988gBjR</a></p>&mdash; Andrew Clark (@acdlite) <a href="https://twitter.com/acdlite/status/909926793536094209">September 18, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

*Tip: Pay attention to the spinning black square.*

We think async rendering is a big deal, and represents the future of React. To make migration to v16.0 as smooth as possible, we're not enabling any async features yet, but we're excited to start rolling them out in the coming months. Stay tuned!

## Installation

React v16.0.0 is available on the npm registry.

To install React 16 with Yarn, run:

```bash
yarn add react@^16.0.0 react-dom@^16.0.0
```

To install React 16 with npm, run:

```bash
npm install --save react@^16.0.0 react-dom@^16.0.0
```

We also provide UMD builds of React via a CDN:

```html
<script crossorigin src="https://unpkg.com/react@16/umd/react.production.min.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.production.min.js"></script>
```

Refer to the documentation for [detailed installation instructions](/react/docs/installation.html).

## Upgrading

Although React 16 includes significant internal changes, in terms of upgrading, you can think of this like any other major React release. We've been serving React 16 to Facebook and Messenger.com users since earlier this year, and we released several beta and release candidate versions to flush out additional issues. With minor exceptions, **if your app runs in 15.6 without any warnings, it should work in 16.**

### New deprecations

Hydrating a server-rendered container now has an explicit API. If you're reviving server-rendered HTML, use [`ReactDOM.hydrate`](/react/docs/react-dom.html#hydrate) instead of `ReactDOM.render`. Keep using `ReactDOM.render` if you're just doing client-side rendering.

### React Addons

As previously announced, we've [discontinued support for React Addons](/react/blog/2017/04/07/react-v15.5.0.html#discontinuing-support-for-react-addons). We expect the latest version of each addon (except `react-addons-perf`; see below) to work for the foreseeable future, but we won't publish additional updates.

Refer to the previous announcement for [suggestions on how to migrate](/react/blog/2017/04/07/react-v15.5.0.html#discontinuing-support-for-react-addons).

`react-addons-perf` no longer works at all in React 16. It's likely that we'll release a new version of this tool in the future. In the meantime, you can [use your browser's performance tools to profile React components](/react/docs/optimizing-performance.html#profiling-components-with-the-chrome-performance-tab).

### Breaking changes

React 16 includes a number of small breaking changes. These only affect uncommon use cases and we don't expect them to break most apps.

* React 15 had limited, undocumented support for error boundaries using `unstable_handleError`. This method has been renamed to `componentDidCatch`. You can use a codemod to [automatically migrate to the new API](https://github.com/reactjs/react-codemod#error-boundaries).
* `ReactDOM.render` and `ReactDOM.unstable_renderIntoContainer` now return null if called from inside a lifecycle method. To work around this, you can use [portals](https://github.com/facebook/react/issues/10309#issuecomment-318433235) or [refs](https://github.com/facebook/react/issues/10309#issuecomment-318434635).
* `setState`:
  * Calling `setState` with null no longer triggers an update. This allows you to decide in an updater function if you want to re-render.
  * Calling `setState` directly in render always causes an update. This was not previously the case. Regardless, you should not be calling setState from render.
  * `setState` callbacks (second argument) now fire immediately after `componentDidMount` / `componentDidUpdate` instead of after all components have rendered.
* When replacing `<A />` with `<B />`,  `B.componentWillMount` now always happens before  `A.componentWillUnmount`. Previously, `A.componentWillUnmount` could fire first in some cases.
* Previously, changing the ref to a component would always detach the ref before that component's render is called. Now, we change the ref later, when applying the changes to the DOM.
* It is not safe to re-render into a container that was modified by something other than React. This worked previously in some cases but was never supported. We now emit a warning in this case. Instead you should clean up your component trees using `ReactDOM.unmountComponentAtNode`. [See this example.](https://github.com/facebook/react/issues/10294#issuecomment-318820987)
* `componentDidUpdate` lifecycle no longer receives `prevContext` param. (See [#8631](https://github.com/facebook/react/issues/8631))
* Shallow renderer no longer calls `componentDidUpdate` because DOM refs are not available. This also makes it consistent with `componentDidMount` (which does not get called in previous versions either).
* Shallow renderer does not implement `unstable_batchedUpdates` anymore.

### Packaging

* There is no `react/lib/*` and `react-dom/lib/*` anymore. Even in CommonJS environments, React and ReactDOM are precompiled to single files (“flat bundles”). If you previously relied on undocumented React internals, and they don’t work anymore, let us know about your specific case in a new issue, and we’ll try to figure out a migration strategy for you.
* There is no `react-with-addons.js` build anymore. All compatible addons are published separately on npm, and have single-file browser versions if you need them.
* The deprecations introduced in 15.x have been removed from the core package. `React.createClass` is now available as `create-react-class`, `React.PropTypes` as `prop-types`, `React.DOM` as `react-dom-factories`, `react-addons-test-utils` as `react-dom/test-utils`, and shallow renderer as `react-test-renderer/shallow`. See [15.5.0](https://facebook.github.io/react/blog/2017/04/07/react-v15.5.0.html) and [15.6.0](https://facebook.github.io/react/blog/2017/06/13/react-v15.6.0.html) blog posts for instructions on migrating code and automated codemods.
* The names and paths to the single-file browser builds have changed to emphasize the difference between development and production builds. For example:
    * `react/dist/react.js` → `react/umd/react.development.js`
    * `react/dist/react.min.js` → `react/umd/react.production.min.js`
    * `react-dom/dist/react-dom.js` → `react-dom/umd/react-dom.development.js`
    * `react-dom/dist/react-dom.min`.js → `react-dom/umd/react-dom.production.min.js`

## JavaScript Environment Requirements

React 16 depends on the collection types [Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) and [Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set). If you support older browsers and devices which may not yet provide these natively (e.g. IE < 11), consider including a global polyfill in your bundled application, such as [core-js](https://github.com/zloirock/core-js) or [babel-polyfill](https://babeljs.io/docs/usage/polyfill/).

A polyfilled environment for React 16 using core-js to support older browsers might look like:

```js
import 'core-js/es6/map';
import 'core-js/es6/set';

import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

React also depends on `requestAnimationFrame` (even in test environments). A simple shim for test environments would be:

```js
global.requestAnimationFrame = function(callback) {
  setTimeout(callback, 0);
};
```

## Acknowledgments

As always, this release would not have been possible without our open source contributors. Thanks to everyone who filed bugs, opened PRs, responded to issues, wrote documentation, and more!

Special thanks to our core contributors, especially for their heroic efforts over the past few weeks during the prerelease cycle: [Brandon Dail](https://twitter.com/aweary), [Jason Quense](https://twitter.com/monasticpanic), [Nathan Hunzaker](https://twitter.com/natehunzaker), and [Sasha Aickin](https://twitter.com/xander76).
