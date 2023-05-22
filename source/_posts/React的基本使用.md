---
title: React的基本使用
date: 2022-06-07 18:55:32
tags:
- JavaScript
- React
categories: Web前端
cover: https://s2.loli.net/2022/06/08/G83TBvkaOQxcqj2.png
---

# React简介

- React 是一个用于构建用户界面（UI，对咱们前端来说，简单理解为：HTML 页面）的 JavaScript 库  
- 如果从mvc的角度来看，React仅仅是视图层（V）的解决方案。也就是只负责视图的渲染，并非提供了完整了M和C的功能
- react --> react-dom --> react-router --> redux（React全家桶）
- React 起源于 Facebook 内部项目（News Feed，2011），后又用来架设 Instagram 的网站（2012），并于 2013 年 5 月开源
- React 是最流行的前端开发框架之一，其他框架：Vue、Angular

## 特点

### 声明式UI

> 你只需要描述UI（HTML）看起来是什么样的，就跟写HTML一样

```jsx
const jsx = (<div className="box">Hello React</div>)
```

### 组件化

- 组件是react中**最重要**的内容
- 组件用于表示页面中的部分内容
- 组合、复用多个组件，就可以实现完整的页面功能

### 学习一次，随处使用

- 使用react/rect-dom可以开发Web应用
- 使用react/react-native可以开发移动端原生应用（react-native）  RN   安卓 和 ios应用    flutter
- 使用react可以开发VR（虚拟现实）应用（react360）

### 从我的角度看React

- 工资高、大厂必备（阿里在用）
- 工资高、大厂必备（字节跳动在用）
- 工资高、大厂必备（百度、腾讯、京东、蚂蚁金服、拼多多、美团、外企、银行等都在用）

## React脚手架（CLI）

### 什么是脚手架

- 脚手架：为了保证各施工过程顺利进行而搭设的工作平台
- 对于前端项目开发来说，脚手架是为了保证前端项目开发过程顺利进行而搭设的开发平台
- 脚手架的意义：
  - 现代的前端开发日趋成熟，需要依赖于各种工具，比如，webpack、babel、eslint、sass/less/postcss等
  - 工具配置繁琐、重复，各项目之间的配置大同小异
  - 开发阶段、项目发布，配置不同
    - 项目开始前，帮你搭好架子，省去繁琐的 webpack 配置
    - 项目开发时，热更新、格式化代码、git 提交时自动校验代码格式等
    - 项目发布时，一键自动打包，包括：代码压缩、优化、按需加载等

### 使用React脚手架创建项目

- 命令：`npx create-react-app <项目名称>`
  - npx create-react-app 是固定命令，`create-react-app` 是 React 脚手架的名称
- 启动项目：`yarn start` or `npm start` 
- `npx` 是 npm v5.2 版本新添加的命令，用来简化 npm 中工具包的使用
  - 原始：
    1. 全局安装`npm i -g create-react-app` 
    2. 在通过脚手架的命令来创建 React 项目
  - 现在：npx 调用最新的 create-react-app 直接创建 React 项目

### 项目目录结构说明和调整

- 说明：
  - `src` 目录是我们写代码进行项目开发的目录
  - 查看 `package.json` 两个核心库：`react`、`react-dom`（脚手架已经帮我们安装好，我们直接用即可）
- 调整：
  1. 删除 src 目录下的所有文件
  2. 创建 index.js 文件作为项目的入口文件，在这个文件中写 React 代码即可

# React 的基本使用

> 使用步骤

1. 导入react 和 react-dom/client
2. 创建react元素(虚拟DOM)
3. 渲染react元素到页面中

> 导入React and ReactDOM

```javascript
import React,{ Component } from 'react'
import { createRoot } from 'react-dom/client'
```

> 创建React元素

```javascript
// 创建元素
cons element = React.createElement('h1', null, 'Hello React')
```

> 渲染元素到页面

```javascript
// 初始化根容器
const container = createRoot(document.getElementById('root'))
// 渲染
container.render(element)
```

# JSX

## JSX的基本使用

### createElement的问题

- 繁琐不简洁
- 不直观，无法一眼看出所描述的结构
- 不优雅，开发体验不好

### JSX简介 

`JSX`是`JavaScript XML`的简写，表示了在Javascript代码中写XML(HTML)格式的代码

优势：声明式语法更加直观，与HTML结构相同，降低学习成本，提高开发效率。

 **JSX是react的核心内容**

> 注意：*JSX 不是标准的 JS 语法，是 JS 的语法扩展。脚手架中内置的 [@babel/plugin-transform-react-jsx](@babel/plugin-transform-react-jsx) 包，用来解析该语法。*

### 使用步骤

> - 导入react和reactDOM包
>
> - 使用jsx语法创建react元素
> - 把react元素渲染到页面中

> 导入渲染就不掩饰了

```javascript
// 创建元素
const title = <h1 title="哈哈"></h1>
```

## JSX注意点

- 只有在脚手架中才能使用jsx语法
  - 因为JSX需要经过babel的编译处理，才能在浏览器中使用。脚手架中已经默认有了这个配置。
- JSX必须要有一个根节点， `<></>`  `<React.Fragment></React.Fragment>`
- 没有子节点的元素可以使用`/>`结束
- JSX中语法更接近与JavaScript
  - `class` =====> `className`
  - `for`========>  `htmlFor`
- JSX可以换行，如果JSX有多行，推荐使用`()`包裹JSX，防止自动插入分号的bug

## JSX中嵌入JavaScript表达式

> 在react中，一切都是javascript，所以条件渲染完全是通过js来控制的

```javascript
const isLoding = false
const loadData = () => {
  if (isLoding) {
    return <div>数据加载中.....</div>
  } else {
    return <div>数据加载完成，此处显示加载后的数据</div>
  }
}

const title = <div>条件渲染：{loadData()}</div>
```

```javascript
const isLoding = false
const loadData = () => {
  return isLoding ? (
    <div>数据加载中.....</div>
  ) : (
    <div>数据加载完成，此处显示加载后的数据</div>
  )
}
```

```javascript
const isLoding = false
const loadData = () => {
  return isLoding && <div>加载中...</div>
}

const title = <div>条件渲染：{loadData()}</div>
```

## 列表渲染

> 在react中，通过map方法进行列表的渲染

```javascript
const songs = ['温柔', '倔强', '私奔到月球']

const dv = (
  <div>
    <ul>{songs.map(song => <li key={song}>{song}</li>)}</ul>
  </div>
)
```

**注意：列表渲染时应该给重复渲染的元素添加key属性，key属性的值要保证唯一**

**注意：key值避免使用index下标，因为下标会发生改变**

## 样式处理

### 行内样式-style

```javascript
const dv = (
  <div style={{ color: 'red', backgroundColor: 'pink' }}>style样式</div>
)
```

### 类名-className

```javascript
// 导入样式
import './base.css'
const dv = <div className="title">style样式</div>
```

## JSX总结

- JSX是React的核心内容
- JSX表示在JS代码中书写HTML结构，是React声明式的体现
- 使用JSX配合嵌入的JS表达式，条件渲染，列表渲染，可以渲染任意的UI结构
- 结果使用className和style的方式给JSX添加样式
- React完全利用JS的语言自身的能力来编写UI，而**不是造轮子增强HTML的功能**。（<font color=red>内涵Vue</font>）

# 组件

## 函数组件

> 函数组件：使用JS的函数或者箭头函数创建的组件

- 为了区分普通标签，函数组件的名称必须`大写字母开头`
- 函数组件`必须有返回值`，表示该组件的结构
- 如果返回值为null,表示不渲染任何内容

> 使用函数创建组件

```javascript
function Hello () {
    return (
    	<div>这是我的函数组件</div>
    )
}
```

> 使用箭头函数创建组件

```javascript
const Hello = () => <div>这是一个函数组件</div>
```

> 使用组件

```javascript
// 初始化根容器
const container = createRoot(document.getElementById('root'))
// 渲染
container.render(<Hello />)
```

## 类组件

> 使用ES6的class语法创建组件叫做：类组件

> 类组件要求：
>
> 1. 类组件的名称必须是大写字母开头
> 2. 类组件应该继承`React.Component`父类，从而可以使用父类中提供的方法或者属性
> 3. 类组件必须提供`render`方法
> 4. render方法`必须有返回值`,表示该组件的结构

```javascript
class Hello extends React.Component {
  render() {
    return <div>这是一个类组件</div>
  }
}
```

> 使用

 ```java
// 初始化根容器
const container = createRoot(document.getElementById('root'))
// 渲染
container.render(<Hello />)
 ```

## 将组件提取到单独的js文件中

1. 创建Hello.js
2. 在 Hello.js 中导入React
3. 创建组件（函数 或 类）
4. 在 Hello.js 中导出该组件
5. 在 index.js 中导入 Hello 组件
6. 渲染组件

## 有状态组件和无状态组件

- 函数组件又叫做**无状态组件**   函数组件是不能自己提供数据的，，，，，**木偶组件，静态组件**
- 类组件又叫做**有状态组件  智能组件** 类组件可以自己提供数据，，，，组件内部的状态（数据如果发生了改变，内容会自动的更新）
- 状态（state）即组件的私有数据，当组件的状态发生了改变，页面结构也就发生了改变。
- 函数组件是没有状态的，只负责页面的展示（静态，不会发生变化）性能比较高
- 类组件有自己的状态，负责**更新UI**，只要类组件的数据发生了改变，UI就会发生更新。
- 在复杂的项目中，一般都是由函数组件和类组件共同配合来完成的。

## 类组件的状态

- 状态`state`即数据，是组件内部的`私有数据`,只有在组件内部可以使用
- `state的值是一个对象`,表示一个组件中可以有多个数据
- state的基本使用

> 基础写法

```javascript
class Hello extends React.Component {
  constructor() {
    super()
    // 组件通过state提供数据
    this.state = {
      msg: 'hello react'
    }
  }
  render() {
    return <div>state中的数据--{this.state.msg}</div>
  }
}
```

> 简洁写法

```javascript
class Hello extends React.Component {
  state = {
    msg: 'hello react'
  }
  render() {
    return <div>state中的数据--{this.state.msg}</div>
  }
}
```

## React浏览器调试工具

> 名字叫：react-devtools
>
> 浏览器扩展商店或是百度都能搜到

# 事件处理

> React注册事件与DOM的事件语法非常像
>
> 语法`on+事件名=｛事件处理程序｝` 比如`onClick={this.handleClick}`
>
> 注意：**React事件采用驼峰命名法**，比如`onMouseEnter`, `onClick`

```javascript
class App extends React.Component {
  render() {
    return (
      <div>
        <button onClick={this.handleClick}>点我</button>
      </div>
    )
  }

  handleClick() {
    console.log('点击事件触发了')
  }
}
```

## 事件对象

- 可以通过事件处理程序的参数获取到事件对象
- React 中的事件对象叫做：合成事件（对象）
- 合成事件：兼容所有浏览器，无需担心跨浏览器兼容性问题

> <font color=red size=3>如果函数中有事件对象和其他参数，那么事件对象形参写在参数的最后</font>

## this的指向问题

> 事件处理程序中的this指向的是undefined
>
> render方法中的this指向的而是当前react组件。**只有事件处理程序中的this有问题**

```javascript
class App extends React.Component {
  state = {
    msg: 'hello react'
  }
  handleClick() {
    console.log(this.state.msg)
  }
  render() {
    return (
      <div>
        <button onClick={this.handleClick}>点我</button>
      </div>
    )
  }
}
```

## 解决this的指向问题

### 箭头函数

> 缺点：会把大量的js处理逻辑放到JSX中，将来不容易维护

```javascript
class App extends React.Component {
  state = {
    msg: 'hello react'
  }
  render() {
    return (
      <div>
        <button onClick={() => { console.log(this.state.msg) }>点我</button>
      </div>
    )
  }
}
```

### bind()方法

```javascript
class App extends React.Component {
  state = {
    msg: 'hello react'
  }
  handleClick() {
    console.log(this.state.msg)
  }
  render() {
    return (
      <div>
        <button onClick={this.handleClick.bind(this)}>点我</button>
      </div>
    )
  }
}
```

> 或者

```javascript
class App extends React.Component {
  constructor() {
  	super()
    this.handleClick = this.handleClick.bind(this)
  }
  state = {
    msg: 'hello react'
  }
  handleClick() {
    console.log(this.state.msg)
  }
  render() {
    return (
      <div>
        <button onClick={this.handleClick}>点我</button>
      </div>
    )
  }
}
```

### Class实例方法

> **注意：这个语法是试验性的语法，但是有babel的转义，所以没有任何问题**

```javascript
class App extends React.Component {
  state = {
    msg: 'hello react'
  }

  handleClick = () => {
    console.log(this.state.msg)
  }
  render() {
    return (
      <div>
        <button onClick={this.handleClick}>点我</button>
      </div>
    )
  }
}
```

# setState修改状态

- 组件中的状态是可变的
- 语法`this.setState({要修改的数据})`
- 注意：不要直接修改state中的值，必须通过`this.setState()`方法进行修改
- `setState`的作用
  - 修改state
  - 更新UI
- 思想：数据驱动视图

```javascript
class App extends React.Component {
  state = {
    count: 1
  }
  handleClick() {
    this.setState({
      count: this.state.count + 1
    })
  }
  render() {
    return (
      <div>
        <p>次数: {this.state.count}</p>
        <button onClick={this.handleClick.bind(this)}>点我+1</button>
      </div>
    )
  }
}
```

## 更新数据

> - setState() 是异步更新数据的
> - 注意：使用该语法时，后面的 setState() 不要依赖于前面的 setState()

> 1. 当你调用 setState 的时候，React.js 并不会马上修改 state （为什么）
> 2. 而是把这个对象放到一个更新队列里面
> 3. 稍后才会从队列当中把新的状态提取出来合并到 state 当中，然后再触发组件更新。

> 可以多次调用 setState() ，只会触发一次重新渲染

```javascript
this.state = { count: 1 }
this.setState({
	count: this.state.count + 1
})
console.log(this.state.count) // 1
```

> <font color=red>在使用 React.js 的时候，并不需要担心多次进行 `setState` 会带来性能问题。</font>

## 推荐使用的语法

> - 推荐：使用 `setState((preState) => {})` 语法
> - 参数preState: React.js 会把上一个 `setState` 的结果传入这个函数

> **这种语法依旧是异步的，但是state可以获取到最新的状态，适用于需要调用多次setState**

```javascript
this.setState((preState) => {
    return {
    	count: preState.count + 1
    }
})
console.log(this.state.count) // 1
```

## 第二个参数

> - 场景：在状态更新（页面完成重新渲染）后立即执行某个操作
> - 语法：`setState(updater[, callback])`

```javascript
this.setState(
	(state) => ({}),
	() => {console.log('这个回调函数会在状态更新后立即执行')}
)
```

> 传入props

```javascript
this.setState(
	(state, props) => {},
	() => {
		document.title = '更新state后的标题：' + this.state.count
	}
)
```



# 表单处理

> 开发过程中，经常需要操作表单元素，比如获取表单的值或者是设置表单的值。

react中处理表单元素有两种方式：

- 受控组件
- 非受控组件（DOM操作）

## 受控组件基本概念

- HTML中表单元素是可输入的，即表单用户并维护着自己的可变状态（value）。
- 但是在react中，可变状态通常是保存在state中的，并且要求状态只能通过`setState`进行修改。
- React中将state中的数据与表单元素的value值绑定到了一起，`由state的值来控制表单元素的值`
- 受控组件：**value值受到了react控制的表单元素** 

![受控组件](受控组件.png)

## 受控组件使用步骤

1. 在state中添加一个状态，作为表单元素的value值（控制表单元素的值）
2. 给表单元素添加change事件，设置state的值为表单元素的值（控制值的变化）

```javascript
class App extends React.Component {
  state = {
    msg: 'hello react'
  }

  handleChange = (e) => {
    this.setState({
      msg: e.target.value
    })
  }

  render() {
    return (
      <div>
        <input type="text" value={this.state.msg} onChange={this.handleChange}/>
      </div>
    )
  }
}
```

## 常见的受控组件

- 文本框、文本域、下拉框（操作value属性）
- 复选框（操作checked属性）

```javascript
class App extends React.Component {
  state = {
    usernmae: '',
    desc: '',
    city: "2",
    isSingle: true
  }
 
  handleName = e => {
    this.setState({
      name: e.target.value
    })
  }
  handleDesc = e => {
    this.setState({
      desc: e.target.value
    })
  }
  handleCity = e => {
    this.setState({
      city: e.target.value
    })
  }
  handleSingle = e => {
    this.setState({
      isSingle: e.target.checked
    })
  }

  render() {
    return (
      <div>
        姓名：<input type="text" value={this.state.username} onChange={this.handleName}/>
        <br/>
        描述：<textarea value={this.state.desc} onChange={this.handleDesc}></textarea>
        <br/>
        城市：<select value={this.state.city} onChange={this.handleCity}>
          <option value="1">北京</option>
          <option value="2">上海</option>
          <option value="3">广州</option>
          <option value="4">深圳</option>
        </select>
        <br/>
        是否单身：<input type="checkbox" checked={this.state.isSingle} onChange={this.handleSingle}/>
      </div>
    )
  }
}
```

## 多表单元素受控的优化

问题：每个表单元素都需要一个单独的事件处理程序，处理太繁琐

优化：使用一个事件处理程序处理多个表单元素

步骤

- 给表单元素添加name属性，名称与state属性名相同
- 根据表单元素类型获取对应的值
- 在事件处理程序中通过`[name]`修改对应的state

```javascript
class App extends React.Component {
  state = {
    username: '',
    desc: '',
    city: "2",
    isSingle: true
  }
 
  handleChange = e => {
    let {name, type, value, checked} = e.target
    console.log(name, type, value, checked)
    value = type === 'checkbox' ? checked : value
    console.log(name, value)
    this.setState({
      [name]: value
    })
  }
  render() {
    return (
      <div>
        姓名：<input type="text" name="username" value={this.state.username} onChange={this.handleChange}/>
        <br/>
        描述：<textarea name="desc" value={this.state.desc} onChange={this.handleChange}></textarea>
        <br/>
        城市：<select name="city" value={this.state.city} onChange={this.handleChange}>
          <option value="1">北京</option>
          <option value="2">上海</option>
          <option value="3">广州</option>
          <option value="4">深圳</option>
        </select>
        <br/>
        是否单身：<input type="checkbox" name="isSingle" checked={this.state.isSingle} onChange={this.handleChange}/>
      </div>
    )
  }
}
```

## 非受控组件（Ref获取）

> 非受控组件借助于ref，使用原生DOM的方式来获取表单元素的值

> 使用步骤

- 调用`React.createRef()`方法创建一个ref

```js
constructor() {
  super()
  this.txtRef = React.createRef()
}
```

- 将创建好的ref对象添加到文本框中

```js
<input type="text" ref={this.txtRef}/>
```

- 通过ref对象获取文本框的值

```js
handleClick = () => {
  console.log(this.txtRef.current.value)
}
```

非受控组件用的不多，推荐使用受控组件

# 综合案例（列表展示）

- 列表展示
- 发表评论
- 清空评论

> css

```css
.app {
  width: 300px;
  padding: 10px;
  border: 1px solid #999;
}

.user {
  width: 100%;
  box-sizing: border-box;
  margin-bottom: 10px;
}

.content {
  width: 100%;
  box-sizing: border-box;
  margin-bottom: 10px;
}

.no-comment {
  text-align: center;
  margin-top: 30px;
}
```

> javascript

```javascript
import React from 'react'
import { createRoot } from 'react-dom/client'

import './index.css'

/**
 * 展示，
 * 清空，
 * 发表，
 * 删除，
 * 没有数据，
 */
class App extends React.Component {
  state = {
    name: '',
    context: '',
    list: [
      {
        id: 1,
        name: '刘德华',
        context: '冰雨',
      },
      {
        id: 2,
        name: ' 张学友',
        context: '我爱你',
      },
    ],
  }
  render() {
    return (
      <div className="app">
        <div>
          <input
            className="user"
            type="text"
            placeholder="请输入评论人"
            value={this.state.name}
            onChange={this.changeHandler}
            name="name"
          />
          <br />
          <textarea
            className="content"
            cols="30"
            rows="10"
            placeholder="请输入评论内容"
            value={this.state.context}
            onChange={this.changeHandler}
            name="context"
          />
          <br />
          <button onClick={this.send}>发表评论</button>
          <button onClick={this.clearList}>清空评论</button>
        </div>
        {this.renderList()}
      </div>
    )
  }

  clearList = () => {
    this.setState({
      list: [],
    })
  }
  send = () => {
    const { name, context, list } = this.state
    if (!name || !context) {
      return alert('请输入评论人和评论内容')
    }
    this.setState({
      list: [
        {
          id: list[list.length - 1].id + 1,
          name,
          context,
        },
        ...list,
      ],
      name: '',
      context: '',
    })
  }
  changeHandler = (e) => {
    const { name, value } = e.target
    this.setState({
      [name]: value,
    })
  }

  renderList() {
    if (this.state.list.length === 0) {
      return <div className="no-comment">暂无评论</div>
    }
    return (
      <ul>
        {this.state.list.map((item) => (
          <li key={item.id}>
            <h3>评论人：{item.name}</h3>
            <p>评论内容：{item.context}</p>
            {/* this.del.bind(this, item.id) */}
            <button
              onClick={() => {
                this.del(item.id)
              }}
            >
              删除
            </button>
          </li>
        ))}
      </ul>
    )
  }

  del = (id) => {
    console.log('111')
    this.setState({
      list: this.state.list.filter((item) => item.id !== id),
    })
  }
}

// 渲染组件
const app = createRoot(document.getElementById('root'))
app.render(<App />)
```

# 组件通讯

## props

- 组件是封闭的，要接收外部数据应该通过props来实现
- **props的作用：接收传递给组件的数据**
- 传递数据：给组件标签添加属性
- 接收数据：函数组件通过参数props接收数据，类组件通过this.props接收数据

### 函数组件通讯

> 子组件

```javascript
function Hello(props) {
    console.log(props)
    return (
    	<div>接收到数据：{props.name}</div>
    )
}
```

> 父组件

```html
<Hello name="jack" age={19} />
```

### 类组件通讯

> 子组件

```javascript
class Hello extends React.Component {
    render() {
        return (
        	<div>接收到的数据：{this.props.age}</div>
        )
    }
}
```

> 父组件

```html
<Hello name="jack" age={19} />
```

### props的特点

- 可以给组件传递任意类型的数据
- props是只读的，不允许修改props的数据
- 注意：在类组件中使用的时候，**需要把props传递给super()**，否则构造函数无法获取到props
- 

```javascript
class Hello extends React.Component {
    constructor(props) {
        // 推荐将props传递给父类构造函数
        super(props)
    }
    render() {
    	return <div>接收到的数据：{this.props.age}</div>
    }
}
```

## 组件通讯三种方式

### 父传子

1. 父组件提供要传递的state数据
2. 给子组件标签添加属性，值为 state 中的数据
3. 子组件中通过 props 接收父组件中传递的数据

> 父组件提供数据并且传递给子组件

```javascript
class Parent extends React.Component {
    state = { lastName: '王' }
    render() {
        return (
            <div>
                传递数据给子组件：<Child name={this.state.lastName} />
            </div>
        )
    }
}
```

> 子组件接收数据

```javascript
function Child(props) {
	return <div>子组件接收到数据：{props.name}</div>
}
```

### 子传父

思路：利用回调函数，父组件提供回调，子组件调用，将要传递的数据作为回调函数的参数。

1. 父组件提供一个回调函数（用于接收数据）
2. 将该函数作为属性的值，传递给子组件
3. 子组件通过 props 调用回调函数
4. 将子组件的数据作为参数传递给回调函数

> 父组件提供函数并且传递给子组件

```javascript
class Parent extends React.Component {
    getChildMsg = (msg) => {
        console.log('接收到子组件数据', msg)
    }
    render() {
        return (
            <div>
            	子组件：<Child getMsg={this.getChildMsg} />
            </div>
        )
    }
}
```

> 子组件接收函数并且调用

```javascript
class Child extends React.Component {
    state = { childMsg: 'React' }
    handleClick = () => {
    	this.props.getMsg(this.state.childMsg)
    }
    return (
    	<button onClick={this.handleClick}>点我，给父组件传递数据</button>
    )
}
```

<font color=red size=5>注意：回调函数中 this 指向问题</font>

### 兄弟组件（状态提升概念）

- 将共享状态提升到最近的公共父组件中，由公共父组件管理这个状态
- 思想：**状态提升**
- 公共父组件职责：
  - 提供共享状态 
  - 提供操作共享状态的方法
- 要通讯的子组件只需通过 props 接收状态或操作状态的方法

> 状态提升之前

![状态提升之前](状态提升01.png)

> 状态提升之后

![状态提升之后](状态提升02.png)

## 组件通讯-Context

### 基本概念

> 思考：App 组件要传递数据给 Child 组件，该如何处理？

> 使用 props 一层层组件往下传递（繁琐）

![props层层传递](props层层传递.png)

> 更好的姿势：使用 Context
>
> 作用：跨组件传递数据（比如：主题、语言等）

![props夸组件传递](props跨组件传递.png)

### 实现方式

> 调用 React. createContext() 创建 Provider（提供数据） 和 Consumer（消费数据） 两个组件。

```javascript
const { Provider, Consumer } = React.createContext()
```

> 使用 Provider 组件作为父节点。

```jsx
<Provider>
    <div className="App">
    	<Child1 />
    </div>
</Provider>
```

> 设置 value 属性，表示要传递的数据。

```jsx
<Provider value="pink">
```

> 调用 Consumer 组件接收数据。

```jsx
<Consumer>
	{data => <span>data参数表示接收到的数据 -- {data}</span>}
</Consumer>
```

总结：

1. 如果两个组件是远方亲戚（比如，嵌套多层）可以使用Context实现组件通讯
2. Context提供了两个组件：Provider 和 Consumer
3. Provider组件：用来提供数据
4. Consumer组件：用来消费数据

# Props深入使用

## Children属性

> `props.children` 这个属性和vue中的插槽类似

children属性：表示该组件的子节点，只要组件有子节点，props就有该属性

children 属性与普通的props一样，值可以是任意值（文本、React元素、组件，甚至是函数）

```javascript
function Hello(props) {
  return (
    <div>
      该组件的子节点：{props.children}
    </div>
  )
}
```

```jsx
<Hello>我是子节点</Hello>
```

## Props校验

目的：校验接收的props的数据类型，增加组件的健壮性

对于组件来说，props是外来的，无法保证组件使用者传入什么格式的数据

如果传入的数据格式不对，可能会导致组件内部报错。**组件的使用者不能很明确的知道错误的原因。**

> rops校验允许在创建组件的时候，就约定props的格式、类型等

> 作用：规定接收的props的类型必须为数组，如果不是数组就会报错，增加组件的健壮性。

![props校验](props校验.png)

### 使用步骤

1. 导入 prop-types 包
2. 使用组件名.propTypes = {} 来给组件的props添加校验规则
3. 校验规则通过 PropTypes 对象来指定

```javascript
import PropTypes from 'prop-types'
function App(props) {
    return (
    	<h1>Hi, {props.colors}</h1>
    )
}
App.propTypes = {
    // 约定colors属性为array类型
    // 如果类型不对，则报出明确错误，便于分析错误原因
    colors: PropTypes.array
}
```

### 约束规则

1. 常见类型：array、bool、func、number、object、string
2. React元素类型：element
3. 必填项：isRequired
4. 特定结构的对象：shape({})

```javascript
// 函数类型
optionalFunc: PropTypes.func,
// 必选
requiredFunc: PropTypes.func.isRequired,
// 特定结构的对象（自定义）
optionalObjectWithShape: PropTypes.shape({
	color: PropTypes.string,
	fontSize: PropTypes.number
})
```

## Props默认值

> 场景：分页组件  每页显示条数
> 作用：给 props 设置默认值，在未传入 props 时生效

```javascript
function App(props) {
    return (
        <div>
            此处展示props的默认值：{props.pageSize}
        </div>
    )
}
// 设置默认值
App.defaultProps = {
	pageSize: 10
}
// 不传入pageSize属性
<App />
```

## 类的静态属性static

- 实例成员: 通过实例调用的属性或者方法，，，叫做实例成员（属性或者方法）
- 静态成员：通过类或者构造函数本身才能访问的属性或者方法

> 如果学过 `Java` 的同学，对静态方法、属性就会比较属性

```javascript
class Person {
   name = 'zs',
   static age = 18
   sayHi() {
       console.log('哈哈')
   }
   static goodBye() {
       console.log('byebye')
   }
}

const p = new Person()

console.log(p.name) // 由于name在原型链中，所以可以拿到
p.sayHi()

console.log(Person.age) // 类的静态方法，必须要使用 类名.方法 | 属性 调用
Person.goodBye()
```

# 组件生命周期

## 生命周期整体说明

- 每个阶段的执行时机
- 每个阶段钩子函数的执行顺序
- 每个阶段钩子函数的作用
- [详细信息点这里](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

![生命周期整体说明](组件生命周期.png)

## 挂载阶段

> 执行时机：组件创建时（页面加载的时候）

> 执行顺序（如图所示）

![挂载阶段执行顺序](挂载阶段执行顺序.png)

| 钩子函数          | 触发时机                  | 作用                                     |
| ----------------- | ------------------------- | ---------------------------------------- |
| constructor       | 创建组件时，最先执行      | 1. 初始化state  2. 创建Ref等             |
| render            | 每次组件渲染都会触发      | 渲染UI（**注意： 不能调用setState()** ） |
| componentDidMount | 组件挂载（完成DOM渲染）后 | 1. 发送网络请求   2.DOM操作              |

## 更新阶段

> 执行时机：组件状态更新时
>
> 1. setState()
> 2. forceUpdate() --> 强制更新
> 3. 组件接收到新的props（props更新）
>
> 三者任意一种变化，组件就会重新渲染

> 执行顺序

![更新阶段执行顺序](更新阶段执行顺序.png)

## 卸载阶段

> 执行时机：组件从页面中消失

| 钩子函数             | 触发时机                 | 作用                                             |
| -------------------- | ------------------------ | ------------------------------------------------ |
| componentWillUnmount | 组件卸载（从页面中消失） | 执行清理工作（比如：清理定时器，清除挂载函数等） |

# TodosMVC（附完整代码）

> 这个是比较好的基础训练demo

## Demo结构

> 结构如图

![TodosMVC结构图](Demo结构图.png)

> index.js

```javascript
import { Component } from 'react'
import { createRoot } from 'react-dom/client'
import './styles/base.css'
import './styles/index.css'

// 导入组件
import TodoHeader from './components/TodoHeader'
import TodoMain from './components/TodoMain'
import TodoFooter from './components/TodoFooter'

class App extends Component {
  state = {
    list: [
      {
        id: 1,
        name: '吃饭',
        done: false,
      },
      {
        id: 2,
        name: '睡觉',
        done: true,
      },
      {
        id: 3,
        name: '给廖狗两个大比兜',
        done: false,
      },
    ],
    // 有三个编辑状态，分别是：all active completed
    type: 'all',
  }
  render() {
    const { list, type } = this.state
    return (
      <>
        <section className="todoapp">
          <TodoHeader addTodo={this.addTodo} />
          <TodoMain
            list={list}
            delTodoById={this.delTodoById}
            updateDoneById={this.updateDoneById}
            editTodo={this.editTodo}
            type={type}
            checkedAll={this.checkedAll}
          />
          <TodoFooter
            list={list}
            clearTodo={this.clearTodo}
            type={type}
            changeType={this.changeType}
          />
        </section>
      </>
    )
  }

  delTodoById = (id) => {
    this.setState({
      list: this.state.list.filter((item) => item.id !== id),
    })
  }
  updateDoneById = (id) => {
    // 需要根据id把对应的done值取反
    this.setState({
      list: this.state.list.map((item) => {
        if (item.id === id) {
          item.done = !item.done
        }
        return item
      }),
    })
  }

  addTodo = (name) => {
    this.setState({
      list: [
        {
          name,
          id: Date.now(),
          done: false,
        },
        ...this.state.list,
      ],
    })
  }
  editTodo = (id, name) => {
    this.setState({
      list: this.state.list.map((item) => {
        if (item.id === id) {
          item.name = name
        }
        return item
      }),
    })
  }

  clearTodo = () => {
    this.setState({
      list: this.state.list.filter((item) => !item.done),
    })
  }

  changeType = (type) => {
    this.setState({
      type,
    })
  }

  checkedAll = (check) => {
    this.setState({
      list: this.state.list.map((item) => {
        return {
          ...item,
          done: check,
        }
      }),
    })
  }

  componentDidMount() {
    this.setState({
      list: JSON.parse(localStorage.getItem('todos') || '[]'),
    })
  }

  componentDidUpdate() {
    localStorage.setItem('todos', JSON.stringify(this.state.list))
  }
}

const app = createRoot(document.getElementById('root'))
app.render(<App />)

```

> TodoHeader.jsx

```jsx
import React, { Component } from 'react'

class TodoHeader extends Component {
  state = {
    name: '',
  }
  render() {
    return (
      <div>
        <header className="header">
          <h1>todos</h1>
          <input
            className="new-todo"
            placeholder="你需要做什么?"
            autoFocus=""
            value={this.state.name}
            onChange={(e) => {
              this.setState({ name: e.target.value })
            }}
            onKeyUp={this.addTodo}
          />
        </header>
      </div>
    )
  }

  addTodo = (e) => {
    if (e.keyCode !== 13) return
    if (!this.state.name.trim()) {
      return alert('名称不能为空')
    }
    // 添加todo
    console.log(this.state.name)
    this.props.addTodo(this.state.name)
    // 清空输入框
    this.setState({ name: '' })
  }
}

export default TodoHeader

```

> TodoMain.jsx

```jsx
import React, { Component } from 'react'
import TodoItem from './TodoItem'
class TodoMain extends Component {
  render() {
    const { list, type } = this.props
    let showList = []

    if (type === 'active') {
      showList = list.filter((item) => !item.done)
    } else if (type === 'completed') {
      showList = list.filter((item) => item.done)
    } else {
      showList = list
    }
    return (
      <div>
        <section className="main">
          <input
            id="toggle-all"
            className="toggle-all"
            type="checkbox"
            checked={list.every((item) => item.done)}
            onChange={this.handleChangeAll}
          />
          <label htmlFor="toggle-all">Mark all as complete</label>
          <ul className="todo-list">
            {showList.map((item) => {
              return <TodoItem {...this.props} key={item.id} item={item} />
            })}
          </ul>
        </section>
      </div>
    )
  }

  // 选中所有的todo
  handleChangeAll = (e) => {
    this.props.checkedAll(e.target.checked)
  }
}

export default TodoMain
```

> TodoItem.jsx

```jsx
import React, { Component } from 'react'
import classNames from 'classnames'

export default class TodoItem extends Component {
  state = {
    // 当前双击的todo的id
    currentId: '',
    // 记录当前双击的名字
    currentName: '',
  }
  inputRef = React.createRef()

  render() {
    const { item } = this.props
    return (
      <li
        key={item.id}
        className={classNames({
          completed: item.done,
          editing: item.id === this.state.currentId,
        })}
      >
        <div className="view">
          <input
            className="toggle"
            type="checkbox"
            checked={item.done}
            onChange={this.updateDone.bind(this, item.id)}
          />
          <label onDoubleClick={this.showEdit.bind(this, item.id, item.name)}>
            {item.name}
          </label>
          <button
            className="destroy"
            onClick={this.delTodo.bind(this, item.id)}
          />
        </div>
        <input
          className="edit"
          value={this.state.currentName}
          onChange={(e) => {
            this.setState({ currentName: e.target.value })
          }}
          onKeyUp={this.handleKeyUp}
          ref={this.inputRef}
          onBlur={() => {
            this.setState({
              currentId: '',
              currentName: '',
            })
          }}
        />
      </li>
    )
  }

  // 删除todo
  delTodo = (id) => {
    this.props.delTodoById(id)
  }

  // 修改todo的状态（双击展示）
  updateDone = (id) => {
    this.props.updateDoneById(id)
  }

  // 双击展示编辑框
  showEdit = (id, name) => {
    this.setState({
      currentId: id,
      currentName: name,
    })
  }

  // 退出编辑框状态
  handleKeyUp = (e) => {
    console.log(e.keyCode)
    if (e.keyCode === 27) {
      this.setState({
        currentId: '',
        currentName: '',
      })
    }
    if (e.keyCode === 13) {
      // 添加
      this.props.editTodo(this.state.currentId, this.state.currentName)
      // 关闭
      this.setState({
        currentId: '',
        currentName: '',
      })
    }
  }

  // 状态更新时
  componentDidUpdate() {
    // 让输入框有焦点
    this.inputRef.current.focus()
  }
}

```

> TodoFooter.jsx

```jsx
import React, { Component } from 'react'

class TodoFooter extends Component {
  render() {
    const { list } = this.props
    if (list.length === 0) return null

    const leftCount = list.filter((item) => !item.done).length
    const isShow = list.filter((item) => item.done).length > 0
    const { type } = this.props
    return (
      <div>
        <footer className="footer">
          <span className="todo-count">
            <strong>({leftCount})</strong>
            个代办项
          </span>
          <ul className="filters">
            <li>
              <a
                className={type === 'all' ? 'selected' : ''}
                onClick={this.handleClick.bind(this, 'all')}
                href="#/"
              >
                全部
              </a>
            </li>
            <li>
              <a
                className={type === 'active' ? 'selected' : ''}
                onClick={this.handleClick.bind(this, 'active')}
                href="#/active"
              >
                未完成
              </a>
            </li>
            <li>
              <a
                className={type === 'completed' ? 'selected' : ''}
                onClick={this.handleClick.bind(this, 'completed')}
                href="#/completed"
              >
                已完成
              </a>
            </li>
          </ul>
          {isShow && (
            <button className="clear-completed" onClick={this.clearTodo}>
              清除完成的任务
            </button>
          )}
        </footer>
      </div>
    )
  }

  // 清除已完成的任务
  clearTodo = () => {
    this.props.clearTodo()
  }

  // 点击切换展示的数据类型
  handleClick = (type) => {
    this.props.changeType(type)
  }
}

export default TodoFooter

```

# 组件的更新机制

> - setState() 的两个作用： 1. 修改 state 2. 更新组件（UI）
> - 过程：父组件重新渲染时，也会重新渲染子组件。但只会渲染当前组件子树（当前组件及其所有子组件）

![组件的更新机制](组件更新.png)

# 组件的性能优化

> <font color=red size=4>注意：</font>
>
> 1. 功能永远第一
> 2. 其次才是性能优化
>
> 不要捡了芝麻丢了西瓜！！！

## 减轻State

> - 减轻 state：只存储跟组件渲染相关的数据（比如：count / 列表数据 / loading 等）
> - 注意：不用做渲染的数据不要放在 state 中，比如定时器 id等 
> - 对于这种需要在多个方法中用到的数据，应该直接放在 this 中 

```javascript
class Hello extends Component {
    componentDidMount() {
        // timerId存储到this中，而不是state中
        this.timerId = setInterval(() => {}, 2000)
    }
    componentWillUnmount() {
    	clearInterval(this.timerId)
    }
    render() { … }
}
```

> Vue中也同样不要把和渲染无关的数据放到Data中

## 避免不必要的重新渲染

> - 组件更新机制：父组件更新会引起子组件也被更新，这种思路很清晰
> - 问题：子组件没有任何变化时也会重新渲染 （接收到的props没有发生任何的改变）
> - 如何避免不必要的重新渲染呢？
> - 解决方式：使用钩子函数 `shouldComponentUpdate(nextProps, nextState)`
> - 作用：通过返回值决定该组件是否重新渲染，返回 true 表示重新渲染，false 表示不重新渲染
> - 触发时机：更新阶段的钩子函数，组件重新渲染前执行 （shouldComponentUpdate => render）

```javascript
class Hello extends Component {
    // 会传入两个参数，分别是更新后的props和state
    shouldComponentUpdate(nextProps,nextState) {
        // 可以写个条件，判断两次的state或是props是否更新
        // 最后决定是否重新渲染组件
        return false
    }
    render() {…}
}
```

> 随机数案例

> index.js

```javascript
import { Component } from 'react'
import { createRoot } from 'react-dom/client'
import Parent from './Parent'

class App extends Component {
  render() {
    console.log('app render')
    return (
      <div>
        <h1>App</h1>
        <Parent />
        <button
          onClick={() => {
            this.setState({})
          }}
        >
          按钮
        </button>
      </div>
    )
  }
}

const app = createRoot(document.getElementById('root'))
app.render(<App />)
```

> Parent.jsx

```jsx
import React, { Component } from 'react'
import Sun1 from './Sun1'
import Sun2 from './Sun2'

export default class Parent extends Component {
  state = {
    money: 100,
    car: '小黄车',
  }
  render() {
    const { money, car } = this.state
    console.log('parent render')
    return (
      <div>
        <h2>Parent</h2>
        <Sun1 money={money} />
        <Sun2 car={car} />
        <button
          onClick={() => {
            this.setState({ money: 200 })
          }}
        >
          更新money
        </button>
        <button
          onClick={() => {
            this.setState({ car: '五菱 ' })
          }}
        >
          更新car
        </button>
      </div>
    )
  }
}

```

> Sun1.jsx

```jsx
import React, { Component } from 'react'

class Sun1 extends Component {
  state = {
    list: ['阿波罗', '太阳神'],
    current: 0,
  }
  render() {
    console.log('sun1 render')
    return (
      <div>
        Sun2<p>{this.props.money}</p>
        <p>爱车：{this.state.list[this.state.current]}</p>
        <button
          onClick={() => {
            this.setState({
              current: Math.floor(Math.random() * this.state.list.length),
            })
          }}
        >
          随机
        </button>
      </div>
    )
  }

  shouldComponentUpdate(nextProps, nextState) {
    console.log(this.props)
    console.log(nextProps)
    // 返回true或false，决定是否更新
    return this.props.money !== nextProps.money || this.state.current !== nextState.current
  }
}

export default Sun1

```

> Sun2.jsx

```jsx
import React, { Component } from 'react'

export default class Sun2 extends Component {
  render() {
    console.log('sun2  render')
    return (
      <div>
        Sun2<p>{this.props.car}</p>
      </div>
    )
  }
}

```

## 纯组件

> - 纯组件：`React.PureComponent` 与 `React.Component `功能相似
> - 区别：PureComponent 内部自动实现了 shouldComponentUpdate 钩子，不需要手动比较
> - 原理：纯组件内部通过分别 对比 前后两次 props 和 state 的值，来决定是否重新渲染组件

```javascript
class Hello extends React.PureComponent {
    render() {
        return (
        	<div>纯组件</div>
        )
    }
}
```

> <font color=red>**只有在性能优化的时候可能会用到纯组件，不要所有的组件都使用纯组件，因为纯组件需要消耗性能进行对比**</font>

## 纯组件比较-值类型

> - 说明：纯组件内部的对比是 shallow compare（浅层对比）
> - 对于值类型来说：比较两个值是否相同（直接赋值即可，没有坑）

```javascript
let number = 0
let newNumber = number
newNumber = 2
console.log(number === newNumber) // false
```

```javascript
state = { number: 0 }
setState({
  number: Math.floor(Math.random() * 3)
})
// PureComponent内部对比：
最新的state.number === 上一次的state.number // false，重新渲染组件
```

## 纯组件比较-引用类型

> - 说明：纯组件内部的对比是 shallow compare（浅层对比）
> - 对于引用类型来说：只比较对象的引用（地址）是否相同
>
> 

```javascript
const obj = { number: 0 }
const newObj = obj
newObj.number = 2
console.log(newObj === obj) // true
```

```javascript
state = { obj: { number: 0 } }
// 错误做法
state.obj.number = 2
setState({ obj: state.obj })
// PureComponent内部比较：
最新的state.obj === 上一次的state.obj // true，不重新渲染组件
```

### 纯组件的最佳实践

> **注意：state 或 props 中属性值为引用类型时，应该创建新数据，不要直接修改原数据！**

```javascript
// 正确！创建新数据
const newObj = {...state.obj, number: 2}
setState({ obj: newObj })

// 不要用数组的push / unshift 等直接修改当前数组的的方法
// 而应该用 concat 或 slice 等这些返回新数组的方法
this.setState({
	list: [...this.state.list, {新数据}]
})
```

# React路由

## 路由基本介绍

> 现代的前端应用大多都是 SPA（单页应用程序），也就是只有一个 HTML 页面的应用程序。因为它的用户体验更好、对服务器的压力更小，所以更受欢迎。为了有效的使用单个页面来管理原来多页面的功能，前端路由应运而生。
>
> - 前端路由的功能：让用户从一个视图（页面）导航到另一个视图（页面）
> - 前端路由是一套映射规则，在React中，是 URL路径 与 组件 的对应关系  
> - 使用React路由简单来说，就是配置 路径和组件（配对）
>
> - 想要实现单页应用程序（SPA），就必须使用到路由  react-router
> - 官网：[react-router](

## 基本使用

> 安装

```powershell
yarn add react-router-dom
```

> `react-router-dom`这个包提供了几个核心的组件

```javascript
import { HashRouter, Routes, Route, Link } from 'react-router-dom'
```

> 基本使用

```jsx
import { PureComponent } from 'react'
import ReactDOM from 'react-dom/client'
import Home from './pages/Home'
import MyMusic from './pages/MyMusic'
import MyFriends from './pages/MyFriends'

// 导入三个核心组件
import { Routes, HashRouter as Router, Route, NavLink } from 'react-router-dom'
class App extends PureComponent {
  render() {
    return (
      <Router>
        <div>
          <ul>
            <li>
              {/* 最终渲染成a标签 */}
              <NavLink to="/home">首页</NavLink>
            </li>
            <li>
              <NavLink to="/mymusic">我的音乐</NavLink>
            </li>
            <li>
              <NavLink to="/myfriends">我的朋友</NavLink>
            </li>
          </ul>
          <Routes>
            <Route path="/home" element={<Home />} />
            <Route path="/mymusic/:id" element={<MyMusic />} />
            <Route path="/myfriends" element={<MyFriends />} />
          </Routes>
        </div>
      </Router>
    )
  }
}

const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(<App />)

```

## Router详细说明

> - Router 组件：包裹整个应用，一个 React 应用只需要使用一次
> - 两种常用 Router：`HashRouter` 和 `BrowserRouter`
> - HashRouter：使用 URL 的哈希值实现（`localhost:3000/#/first`）
> - BrowserRouter：使用 H5 的 history API 实现（`localhost:3000/first`）

> 推荐写法

```javascript
import { HashRouter as Router, Route, Link } from 'react-router-dom'
```

## Link与NavLink

> `Link`组件最终会渲染成a标签，用于指定路由导航

> - to属性，将来会渲染成a标签的href属性
> - `Link`组件无法实现导航的高亮效果



> NavLink`组件，一个更特殊的`Link`组件，可以用用于指定当前导航高亮

> - to属性，用于指定地址，会渲染成a标签的href属性
> - activeClass: 用于指定高亮的类名，默认`active`
> - exact: 精确匹配，表示必须精确匹配类名才生效

## Route

> - path 的说明
>   - 默认情况下，/能够匹配任意/开始的路径
>   - 如果 path 的路径匹配上了，那么就可以对应的组件就会被 render
>   - 如果 path 没有匹配上，那么会 render null
>   - 如果没有指定 path，那么一定会被渲染
> - exact 的说明， exact 表示精确匹配某个路径
>   - 一般来说，如果路径配置了 /， 都需要配置 exact 属性

## Switch与404

> - 通常，我们会把`Route`包裹在一个`Switch`组件中
> - 在`Switch`组件中，不管有多少个路由规则匹配到了，都只会渲染第一个匹配的组件
> - 通过`Switch`组件非常容易的就能实现404错误页面的提示

```js
<Switch>
  <Route exact path="/" component={Home}/>
  <Route path="/about" component={About}/>
  <Route path="/user" component={User}/>
  <Route component={NoMatch}/>
</Switch>
```

## 嵌套路由的配置

> - 在React中，配置嵌套路由非常的简单，因为`Route`就是一个组件，可以在任意想配置的地方进行配置
> - 但是配置嵌套路由的时候，需要对路径进行处理，**必须**要先匹配到父级路由，才能匹配到子路由
> - **子路由的路径一定要包含父级路径**

```js
// 通过/home可以匹配Home父组件  再通过/list匹配子组件
<Route path="/home/list" component={List} />
```

# 关于React路由

> **路由这里只是粗略提一下,后期会有文章精讲**
