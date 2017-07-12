
## 全面解析children在react中的应用

[原文地址](http://mxstbr.blog/2017/02/react-children-deepdive/)

react的核心是组件。你可以像嵌套html标签那样嵌套react组件，因为它类似于标记，使得编写jsx变得很容易。当我刚学react的时候，我认为使用<code>props.children</code>就是了。我认为自己知道关于children的一切。但是，我错了。因为我正在使用javascript，我可以改变children。我们可以给他们传递特殊的属性，来决定是否渲染他们并且可以按照我们的意愿去操作他们。让我们来深入发掘react中children的潜力吧

### 内容列表

+ 子组件
  + 一切都是孩子
  + 函数也可以作为一个孩子
+ 操作孩子
  + 遍历孩子
  + 统计孩子个数
  + 将chldren转化为数组
  + 限制只有一个children
+ 编辑children
  + 改变children的属性
  + 不可变拷贝元素
+ 总结

### 子组件

我们假设有一个<code><Grid\/></code>组件，它包含<code><Row\/></code>.你可以像下面这样使用:
```javascript
<Grid>
  <Row/>
  <Row/>
  <Row/>
</Grid>

```

![](http://mxstbr.blog/img/react-children-grid-row.png)


这三个Row组件是被当作props.children传递给Grid组件的。使用表达式容器(这是在JSX中那些歪歪的括号的技术术语)，父容器可以渲染他们的孩子:
```javascript
class Grid extends React.Component {
  render() {
    return (
        <div>
        {this.props.children}
        </div>
    );
  }
}
```


父亲也可以直接决定是否渲染孩子或者在渲染前操作他们。例如这个<code><Fullstop\/></code>组件并不渲染任何孩子

```javascript

class Fullstop extends React.Component {
  render() {
    return <h1>Hello world!</h1>
  }
}

```
无论你想传递那个孩子给这个组件，它总是显示“Hello world”, 其他什么都不做。
> 注意：上面例子中的这个<code>\<h1></code>更像html的原始标签，总以“Hello World”渲染他们孩子。

### 一切都可以被当做孩子

在react中后代不一定都是组件，他们可以是任何东西。例如，我们可以传递给<code><Grid\/>组件一些文本作为它的后代，并且他也照常工作地很好。
```javascript
<Grid>Hello world</Grid>
```
![](http://mxstbr.blog/img/react-children-grid-string.png)
[(Live demo)](http://www.webpackbin.com/N1FUPocvz)

JSX将自动移除在一行收尾的空白以及空行。它还将字符串中文字的空白行压缩成一个空格。
这意味下面的例子将渲染出同样的东西:
```javascript
<Grid>Hello world</Grid>
<Grid>
    Hello world!
</Grid>
<Grid>
    Hello
    world!
</Grid>

<Grid>

    Hello world!
</Grid>
```

你也可以混合多种类型的后代，同样也工作地很好:
```javascript
<Grid>
Here is a row:
<Row/>
Here is another row:
<Row/>
</Grid>
```
![](http://mxstbr.blog/img/react-children-grid-mixed.png)
[(Live demo)](http://www.webpackbin.com/E1IpLQ3PM)
<<<<<<< HEAD

### 把函数作为后代

为哦们可以传递任何javascript表达式作为后代。这包括函数。

为了说明这是什么样子，这是一个组件，它执行一个传递给它的函数:
```javascript
class Executioner extends React.Component {
  render() {
    return this.props.children();
  }
}
```
你可以像这样使用这个组件:
```javascript
<Executioner>
  {() => <h1>Hello World!</h1>}
</Executioner>
```
这个特殊的例子并没有什么用处，但是他展示了这个想法。

想象你不得不从服务器上抓取一些数据。你可以采用各种各样的方案，但是使用函数作为后代是一种可行的模式:
```javascript
<Fetch url="api.myself.com">
  {(result) => <p>{result}</p>}
</Fetch>
```
花费一分钟玩一下[这个demo](), 并且看是否可以明白它如何工作。

不要担忧这超过你的理解范围。我想要的是，当你在野外看到这一切时，你并不感到惊讶。使用children，你可以做很多事情。

### 操纵后代

如果你看react官方文档，你讲看到这句话“children are an opaque data structur”。他们基本告诉你props.children
可以是任何类型，例如数据，函数，对象，等等。因此你可以传递任何东西，你可以从不用关心他们。

React提供了一些操纵children的辅助方法，使用这些方法可以很简单无疼地操作children。这些方法在React.children下。

### 遍历后代

React.children.map和React.children.forEach是两个最常用的辅助方法。他们像数组一样工作，除了他们是函数，对象或者其他东西的时候

```javascript
class IngoreFirstChild extends React.Component {
  render () {
    const children = this.props.children;
    return (
      <div>
        {
            React.Children.map(children, (child, i) => {
              if ( i < 1 ) return;
              return child;
            })
        }
      </div>
    );
  }
}
```
这个<IgnoreFirstChild\/>组件当map的时候，会忽略掉第一个，返回其他的
```javascript
<IgnoreFirstChild>
  <h1>First</h1>
  <h1>Second</h1>
</IgnoreFirstChild>
```
![](http://mxstbr.blog/img/react-children-map.png)
[(Live demo)](http://www.webpackbin.com/NyfgFQ2wz)

下面这个例子，我们也可以使用this.props.children.map。但是如果一个人传递一个函数作为它的后代，会发生什么呢? this.props.children将不是一个数组而是一个函数。我们会遇到错误。
![](http://mxstbr.blog/img/react-children-error.png)

使用React.Children.map函数，不会遇到任何问题:
```javascript
<IgnoreFirstChild>
  {() => <h1>First</h1>} // <- Ignored
</IgnoreFirstChild>
```

### 统计后代数量

因为this.props.children可能是任何类型，判断一个组件有多少个后代将会是一件很困难的事。如果传递一个字符串或者函数作为后代，那么将打破this.props.children.length的正常使用;
