
## 全面解析children在react中的应用

[原文地址](http://mxstbr.blog/2017/02/react-children-deepdive/)

react的核心是组件。你可以像嵌套html标签那样嵌套react组件，因为它类似于标记，使得编写jsx变得很容易。

当我刚学react的时候，我认为使用<code>props.children</code>就是了。我认为自己知道关于children的一切。但是，我错了。

因为我正在使用javascript，我可以改变children。我们可以给他们传递特殊的属性，来决定是否渲染他们并且可以按照我们的意愿去操作他们。让我们来深入发掘react中children的潜力吧

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