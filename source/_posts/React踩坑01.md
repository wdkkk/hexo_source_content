---
title: React踩坑01
date: 2017-12-08 11:02:18
tags:
---



## 1. 对React的简单认识

 - React 是一种构建UI的框架 
 - React 专注于视图(V)
 - React 是通过细化的组件构成复杂UI的

<!--more-->

## 2.划重点
 - jsx，就是一种JavaScript(xml)的扩展语法，简单的说就是在JavaScript中用xml语法定义dom结构和行为

```jsx
//简单的jsx语法
const element = <h1>Hello, world!</h1>;
//jsx就是表达式
const name ='world'
const element = <h1>hello,{name}</h1>
//jsx的对象表示
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
//这段jsx的对象表示为
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world'
  }
};
```
要使用jsx需要使用Babel编译，推荐使用ES6+webpack+babel的开发方式

  -  render,将React DOM渲染到根元素上
```jsx
const element = <h1>Hello, world</h1>;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
  根元素将作为React生成的DOM元素的父元素

 - component和props
### *component,组件，是React里最重要最基本的东西*

> Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.(组件可以让你将UI分离为独立的可复用的片段，然后独立的去思考这些片段)

组件分为两种：

 1.Functional Components,函数组件
```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
 2.Class Components， 类组件
```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
组件也可以组合为复合组件
```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

```
**组件的名字必须大写，否则React无法识别，真正的dom元素需要小写**
### *props,属性*，个人理解就是描述这个React组件的“形状”的
在React中，有一条严格的规则，就是所有的React组件相对他们的props来说必须是pure function(纯函数)，大概意思就是React组件的输出只能由他们的props(当然还有state)决定，而不依赖于外界
**props是只读的，一旦赋值后就无法改变它的值**

- State和Lifecycle,状态和生命周期(大佬登场)
### *State,状态*
假如正在使用jQuery，需要显示实时时间，一般的做法就是，获取需要显示时间的dom元素，每秒去获取当前时间，将这个值绑定赋给这个元素
React的做法是，给这个显示时间的组件一个状态，然后每秒去更新这个状态，此时React会自动更新UI
看似差别不大，但当业务场景比较复杂， dom更新频繁的时候，各种获取dom元素赋值的操作让人头皮发麻

举个栗子
```jsx
//给这个显示时间的组件一个date状态，但此时这个状态并没有更新
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```


### *LifeCycle*，生命周期
在这个组件中，要更新显示的时间，需要在这个组件已经插入文档后再去改变(想想学js最开始容易犯的毛病就是在dom元素还没有创建和插入时就去获取这个元素，然后就是undefined...)
生命周期是React内部定义的一系列函数，这些函数称为"lifecycle hooks","生命周期钩子"，通过这些钩子，就可以在组件装载后后改变状态或者组件卸载后释放资源
```jsx
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
//组件装载钩子
  componentDidMount() {
  //改变状态
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
//组件卸载钩子
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```
使用状态时需要注意一些问题：
1. 不要直接改变state的值，而是使用 `setState()`
2. 这个值的改变是异步的
3. React会合并状态的改变
### *Data Flows Down*数据流
React的数据流是单向的，也就是说任何数据(状态)的改变只会影响组件树中位于该组件之下的组件

### *事件处理*
React的事件处理非常类似于DOM0级事件，也就是在元素上直接绑定事件，但是有两点不同：
1. 命名使用`camelCase`命名，比如`onClick`
2. 事件处理需要传递一个函数而不是字符串

举个栗子
```jsx
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```
在React中，React根据W3C的标准重新定义了Event事件，所以它的Event不存在兼容性问题
需要注意的问题：如果使用ES6 class的形式，事件处理函数的this需要格外注意

### *状态提升*
假如现在有多个组件，它们需要对某个状态做出相同的响应，可以将这种状态提升至离这些组件最近的某个父组件。
这个概念看起来很高大上，但个人理解为就是某个子组件需要一个改变状态后的处理函数，这个处理函数其实可以使用父组件通过props传递来的函数，那么这个子组件的状态处理就被提升或者说代理到了父组件上

以上只是刚接触React的个人理解，如果错了，那就错了吧。。

这是学习React中的一些小Demo:

https://github.com/wdkkk/reactdemo