
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
我们有一个后代，“Hello World”，但是.length相反却输出12!

那是为什么我们有React.Children.count:
```javascript
class ChildrenCounter extends React.Component {
  render () {
    return <p>React.Children.count(this.props.children)</p>
  }
}
```
它将返回后代的数量无论他们是什么类型:
```javascript
// Renders "1"
<ChildrenCounter>
  Second!
</ChildrenCounter>
// Renders "2"
<ChildrenCounter>
  <p>First</p>
  <ChildComponent/>
</ChildrenCounter>
// Renders "3“
<ChildrenCounter>
{() => <h1>First</h1>}
Second!
<p>Third!</p>
</ChildrenCounter>
```

### 转化后代为一个数组

作为最后的手段，上面没有任何方法适合你的需求，你可以使用React.Children.toArray转化你的后代为数组. 如果你需要排序他们，这种手段是非常有用的。
```javascript
 class Sort extends React.Component {
   render () {
     const children = React.Children.toArray(this.props.children);
     return <p>{children.sort().join(' ')}</p>
   }
 }

 <Sort>
 // 我们用表达式容器来确认我们的字符串是被当做三个孩子传递进去的，而不是一个字符串
 {'bananas'}{'oranges'}{'apples'}
 </Sort>
```
以上的字符串渲染出排序后的字符串

![](http://mxstbr.blog/img/react-children-apples-bananas-oranges.png)
[(Live demo)](http://www.webpackbin.com/NyE2TQhwz)

### 强制只有一个后代

如果你回头看我们上面的<Executioner\/>组件，它期待只有一个后代被传递进来，并且这个后代必须是函数。
```javascript
class Executioner extends React.Component {
  render () {
    return this.props.children();
  }
}
```
我们可以使用propTypes来尝试限制它这样，代码如下:
```javascript
Executioner.propTypes = {
  children: React.PropTypes.func.isRequired
}
```
这将使控制台输出一段信息，这些信息一部分会被开发者忽略掉，相反，我们可以使用React.Children.only在我们的render方法中!
```javascript
class Executioner extends React.Component {
  render () {
    return React.Children.only(this.props.children)()
  }
}
```
这将返回在this.props.children中仅一个后代。如果有不只一个后代，它将扔出错误。把整个应用程序都磨成一个完美的程序，以避免懒惰的开发者试图破坏我们的组件。

### 修改后代

我们可以将任意组件呈现为子元素，但是仍然从父类中控制它们而不是从我们渲染它们的组件中。为了证实这，我们假设有一个RadioGroup组件，它包含一组RadioButton组件（这个组件将渲染<input type="radio"在一个label标签中）。

这个RadioButton并不从RadioGroup这个组件中渲染，而是被当作一个子元素，这意味着在我们的程序中可以有这样的代码:
···javascript
render () {
  return (
    <RadioGroup>
      <RadioButton value="first"> First </RadioButton>
      <RadioButton value="second"> Second </RadioButton>
      <RadioButton value="third"> Third</RadioButton>
    </RadioGroup>
  );
}
```
上免代码有一个问题。这个input并没有在一个分组里，导致出现下面的现象

![](http://mxstbr.blog/img/react-children-radio-bug.png)

[(Live demo)](http://www.webpackbin.com/Vk-Vt_VawM)

为了把这些input标签放到同一个组里，需要他们有同样的name属性。我们当然可以遍历，然后给每一个单独的RadioButton分配一个name属性。
```javascript
<RadioGroup>
  <RadioButon name="g1" value="first">First</RadioButton>
  <RadioButton name="g1" value="second">Second</RadioButton>
  <RadioButton name="g1" value="third">third</RadioButton> 
</RadioGroup>
```
但那是一个乏味和错误的倾向。我们拥有所有javascript的力量。我们能不能用它来告诉我们的RadioGroup我们想让所有的子元素得到它的名字并且让它自动地处理它?

### 改变子元素的属性

在我们RadioGroup组件中，我们将增加一个绑定过的方法，叫renderChildren，在哪我们将编辑子元素的属性:
```javascript
class RadioGroup extends React.Component {
  constructor() {
    super();
    this.renderChildren = this.renderChildren.bind(this);
  }
  renderChildren() {
    return this.props.children;
  }
  render () {
    return (
      <div className="group">
        {this.renderChildren()}
      </div>
    );
  }
}
```

让我们开始遍历子元素，来得到每个独立的子元素:
```javascript
renderChildren() {
  return React.Children.map(this.props.children, child => {
    return child;
  })
}
```
我们如何可以编辑他们的属性呢？

### 不可变复制元素

这是今天最后一个辅助方法所起的作用。正如名字提示的，React.cloneElement复制一个元素。我们可以传递我们想复制的元素作为第一个参数 并且在第二个参数中传递我们想设置的属性:
```javascript
const cloned = React.cloneElement(element, {
  new: 'yes!'
})
```

这个cloned元素现在已经有了新属性new并被设置为'yes!'.

下面是我们完成RadioGroup的实际逻辑。我们复制每个元素，并设置复制生成的元素this.props.name:
```javascript
renderChildren() {
  return React.Children.map(this.props.children, child => {
    return React.cloneElement(child, {
      name: this.props.name
    })
  })
}
```

最后一步是给我们的RadioGroup组件设置一个唯一name属性值:
```javascript
<RadioGroup name="g1">
  <RadioButton value="first">First</RadioButton>
  <RadioButton value="second">Second</RadioButton>
  <RadioButton value="three"> Three</RadioButton>
</RadioGroup>
```
![](http://mxstbr.blog/img/react-children-radio-done.png)

### 总结
子元素让React组件感觉像标记，而不是脱节的实体。使用JavaScript的强大功能和一些响应帮助函数，我们可以与它们一起创建声明性api，使我们的生活更轻松