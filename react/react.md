# react

```js
const VDON = React.createElement('h1',{id:'title'},'内容')
const { Provider, Consumer } = React.createContext()
textRef = React.createRef()

```



```bash
$ npx create-react-app 项目文件夹名
$ npx create-react-app 项目文件夹名 --template typescript
$ yarn add sass -D
```

```
react是一个用于构建用户界面的 JavaScript 库

1.声明式UI  
2.组件化    
3.学习一次,随处使用  native开发移动端原生应用

虚拟DOM可以摆脱浏览器的束缚   通过虚拟DOM可以跨平台开发
```

## 高效

开发高效    : 组件化,可复用

性能高效   : 虚拟dom+diff算法,最小化页面重绘

# JSX

```
  createElement缺点:

- 繁琐不简洁
- 不直观，无法一眼看出所描述的结构
- 不优雅，开发体验不好
```

`JSX`是`JavaScript XML`的简写，表示了在Javascript代码中写XML(HTML)格式的代码

JSX 不是标准的 JS 语法，是 JS 的语法扩展。脚手架中内置的 [@babel/plugin-transform-react-jsx](@babel/plugin-transform-react-jsx) 包，用来解析该语法。 JSX 创建虚拟DOM更方便  本质是React.createElement()方法的**语法糖**

优势：声明式语法更加直观，与HTML结构相同，降低学习成本，提高开发效率。

```jsx
// 1. 声明式方式
// 虚拟DOM  会被babel自动翻译
const VDON = (
	<h1 id="title">
	  <span>哈哈</span>
  </h1>
)


// 2. 命令式方式
// 虚拟DOM  React.createElement(标签名,标签属性,标签内容) 
const VDON = React.createElement('h1',{id:'title'},React.createElement('span',{},'哈哈')) 


```

## 语法规则

```jsx
// 1. 定义虚拟DOM不要用引号
// 2. js表达式用花括号包裹  表达式? 有输出结果   最简单的判断条件 能被console.log打印的都是表达式
// 3. class类名要替换成className   因为class是es6 关键字
// 4. style内联样式  要写成style = {{key:value}}形式   短横线的要转成小驼峰形式 fontSize
// 5. 只能有一个根标签 <></> 或 <React-Fragment></React-Fragment>
// 6. 标签必须闭合
// 7. 标签首字母,
//   7.1 小写开头,则将该标签转为html同名元素 ,html无对应标签会报错
//   7.1 大写开头,react会去渲染对应的组件,组件没有定义则报错
// 9. for属性要用 htmlFor  如: <label htmlFor="box"></label>
// 10. 多行JSX 用()包裹 , 防止自动加;的bug


const title = 'title'
const text = '哈哈'

const VDOM = (
  <>
    <h1 className="red" id={title}>
      <span style={{ color: 'red' }}> {text}</span>
    </h1>
    <label htmlFor="box">勾选</label>
    <input type="checkbox" id="box" />
  </>
)
// 渲染虚拟DOM到页面
ReactDOM.render(VDOM, document.getElementById('root'))
```

# 组件

```js
// 什么是组件?
+ 组件表示页面中的部分功能
+ 多个组件可以实现完整的页面功能
+ 组件特点：可复用，独立，可组合 
// 创建规则
组件名必须大写开头,为了区分html元素
函数组件必须有返回值,表示该组件的结构
返回值为null,则代表不渲染任何内容
```

## 函数组件

```jsx
// 使用JS的函数或者箭头函数创建的组件  
// 1. 创建函数组件
function MyComponent() {
    return <h2>这是一个函数式组件(适用于简单组件的定义)</h2>
}
// 箭头函数形式
MyComponent = () =>  <h2>这是一个函数式组件(适用于简单组件的定义)</h2>
```

## 类组件

```react
// 要继承React.Component父类
// 要有render方法 并且有返回值  表示该组件的结构
// 1. 创建类组件   
class Teach extends React.Component {
  // 2.hender()上面定义state
  state={
    ....
  }
	render() {
    // 4.return上面定义数据
    ....
    const str = 'jxk'
    // 1.定义jsx
    return <div>类组件</div>
  }
	// 3.定义事件函数
}
```

## 组件状态

```js
状态即组件的私有数据,数据变了会自动同步到视图 , 这样我们只要操作数据即可,不用操作dom

函数组件 => 无状态(静态组件)   

类组件 => 有状态(动态组件)
	
	// 定义state
  state = {
    count: 0
  }
  // 修改count    this.setState({要修改的属性:修改的值})
  clickFn = () => {
    this.setState({
      count: this.state.count + 1
    })
  }
```



# 受控组件

```
常见的受控组件
文本框、文本域、下拉框（操作value属性）
复选框（操作checked属性）


使用步骤:
1. 在state中添加一个状态，作为表单元素的value值（控制表单元素的值）
2. 给表单元素添加change事件，设置state的值为表单元素的值（控制值的变化）
多表单元素:
 + 给表单元素添加name属性，名称与state属性名相同
 + 根据表单元素类型获取对应的值
 + 在事件处理程序中通过`[name]`修改对应的state
```



# 非受控组件

```react
// 非受控组件借助ref, 可以获取到原生DOM或者组件中的属性和方法


// 创建ref
class Teach extends React.Component {
  // 创建ref
  textRef = React.createRef()
	state={
	  ...
	}
	render() {
    // 绑定ref
    return (
    	<input type="text" ref={this.textRef} />
      <button onClick={this.Fn}></button>
    )
  }
	Fn=()=>{
    // 获取到原生DOM
    console.log(this.textRef.current)
  }
}
```

# 组件通讯

```
组件是封闭的,想要组件之间的数据,状态进行传递,就要用到组件通讯
```

## 父传子



```js
// 父组件给子组件传递 在父组件的子组件标签中使用属性传递

// 可以给组件传递任意类型的数据  props是只读的，不允许修改props的数据
<Demo car='奔驰' type={300} fn={()=>{console.log(1)}}><Demo>

// 类组件当中通过this.props接收  
// render外使用需要把props传递给super()
class Demo extends Component {
  constructor(props) {
		super(props)
  }
// render里使用通过this.props即可
  render(){
 	 console.log(this.props)
 	 const {car,type,fn} = this.props
   return (
   	<div>{car}</div>	
   )
  }
}

// 函数组件通过参数接收
function Demo({car,type,fn}){
   return (
   	<div>{car}</div>	
   )
}
```



## 子传父

```react
思路：利用回调函数，父组件提供回调，子组件调用，将要传递的数据作为回调函数的参数。
1. 父组件提供一个回调函数（用于接收数据）
2. 将该函数作为属性的值，传递给子组件
3. 子组件通过 props 调用回调函数
4. 将子组件的数据作为参数传递给回调函数

// 父
class App extends Component {
  render() {
    return (
      <div>
        <h1></h1>
        <hr />
        <Child handleChange={this.changeFn}></Child>
      </div>
    )
  }
  changeFn = name => {
    this.setState({
      username: name
    })
  }
}

// 子 
class Child extends Component {
  state = {
    username: ''
  }
  render() {
    return (
      <div>
        <h3>我是子组件</h3>
        <div>
          妈妈：
          <input
            type="text"
            placeholder="请输入妈妈的名字"
            value={this.state.username}
            onChange={this.Fn}
          />
        </div>
      </div>
    )
  }
  Fn = e => {
    this.setState({
      username: e.target.value
    })
    this.props.handleChange(e.target.value)
  }
}
```

##  createContext跨级通讯

```js
// 跨级组件通讯createContext

// 如果组件嵌套较深使用状态提升的方式 一层层组件往下传递比较繁琐

// 这时可以使用 React. createContext() 创建 Provider（提供数据） 和 Consumer（消费数据） 两个组件。
```

```react
import React, { Component } from 'react'
import ReactDOM from 'react-dom'

// 1.调用 React. createContext() 创建 Provider（提供数据） 和 Consumer（消费数据） 两个组件。
const { Provider, Consumer } = React.createContext()
class App extends Component {
  state = {
    color: 'red'
  }
  render() {
    const { color } = this.state
    return (
      // 2.使用 Provider 组件作为父节点。
      // 3.设置 value 属性，表示要传递的数据 传递多个数据用对象表示
      <Provider value={color}>
        <div>
          <h1>App组件</h1>
          {/* 4.调用Consumer组件 组件内用箭头函数形式表示 参数接收Provider中value的数据 */}
          <Consumer>
            {color => (
              <div>
                <h5 style={{ color }}>Chilr组件</h5>
              </div>
            )}
          </Consumer>
        </div>
      </Provider>
    )
  }
}

ReactDOM.render(<App />, document.getElementById('root'))

```



## prps =>children属性

children属性：表示该组件的子节点，只要组件有子节点，props就有该属性

children 属性与普通的props一样，值可以是任意值（文本、React元素、组件，甚至是函数）

```react
class App extends Component {
  state = {
    title: '对话框',
    context: '内容'
  }
  render() {
    return (
      <div>
        <Dialog title='首页'>
          <div>哈哈</div>
        </Dialog>
      </div>
    )
  }
}

class Dialog extends Component {
  render() {
    // this.props.children  =>  <div>哈哈</div>  
    return (
      <div>
        <div>{this.props.children}</div>
      </div>
    )
  }
}
```

## props校验/默认值

**约束规则**

1. 常见类型：array、bool、func、number、object、string
2. React元素类型：element
3. 必填项：isRequired
4. 特定结构的对象：shape({ })

```react
import { Component } from 'react'
import ReactDOM from 'react-dom'
// 1. 引入
import PropTypes from 'prop-types'

class App extends Component {
  // 状态
  state = {
    list: [{ name: 'hah ' }],
    car: {
      name: 'name',
      type: 1
    }
  }
  render() {
    const { list, car } = this.state
    return (
      <div>
        <Son list={list} car={car}></Son>
      </div>
    )
  }
}
class Son extends Component {
  // 给Son组件增加校验   
  // static要用babel转义才能用 脚手架已安装不需要额外配置
  static propTypes = {
    list: PropTypes.array,
    car: PropTypes.shape({
      name: PropTypes.string.isRequired,
      type: PropTypes.number.isRequired
    })
  }
  // 设置默认值
  static defaultProps = {
    list: [{ name: '哈哈' }],
    car: {
      name: '奔驰',
      type: 3000
    }
  }
  render() {
    const { list, car } = this.props
    return (
      <div>
        <ul>
          {list.map((item, index) => (
            <li key={index}>{item.name + car.name + car.type}</li>
          ))}
        </ul>
      </div>
    )
  }
}

ReactDOM.render(<App />, document.getElementById('root'))

```



# react生命周期

```react
constructor  // 创建组件时最先执行
作用: 1.初始化state 2.创建Ref

render // 每次组件渲染都会触发
作用: 渲染UI (不能调用setState())

componentDidMount  // 组件挂载后
作用: 1.发送网络请求 2.DOM操作

componentDidUpdate // 组件更新后
作用: DOM操作,可以获取到更新后的DOM
```

## 挂载阶段

```react
执行时机: 组件创建时 (页面加载时)

执行顺序: constructor() => render() => componentDidMount()
```

## 更新阶段

```
执行时机: 1. setState() 2. forceUpdate() //强制刷新 3. 组件接收到新的props

执行顺序:  render() => componentDidUpdate()
```

## 卸载阶段

```
componentWillUnmount()
```

# setState

```react
// setState是异步更新数据的
// 第一个参数是对象 或者函数 第二个参数是回调函数
class App extends Component {
  state = {
    count: 0
  }
  render() {
    return (
      <div>
        <h1>{this.state.count}</h1>
        <button onClick={this.Fn1}>第一个参数为对象</button>
        <button onClick={this.Fn2}>第一个参数为函数</button>
      </div>
    )
  }
  Fn1 = () => {
    // setState()
    // 第一个参数
    // 如果是对象,会将多个对象进行合并
    this.setState({ count: this.state.count + 1 })
    this.setState({ count: this.state.count + 1 })
    this.setState({ count: this.state.count + 1 }, () => {
      // 打印0+1   上面的会被覆盖
      console.log(this.state.count)
    })
  }
  Fn2 = () => {
    // setState()
    // 第一个参数  
    // 如果是函数,会依次执行  如果后面的setState需要依赖前面的setState 就要用函数形式
    // 第二个参数是回调函数,因为setState是异步的 在setState第二个参数中可以获取修改过的值 
    this.setState(PreState => ({ count: PreState.count + 1 }))
    this.setState(PreState => ({ count: PreState.count + 1 }))
    this.setState(
      PreState => ({ count: PreState.count + 1 }),
      () => {
        console.log(this.state.count)
      }
    )
     console.log(this.state.count) // setState是异步的 打印不到上面新修改的值
  }
}
```

# 性能优化

## 避免不必要的渲染

## shouldComponentUpdate

```react
import React, { Component } from 'react'
import ReactDOM from 'react-dom'

class App extends Component {
  state = {
    list: ['如花', '凤姐', '乔碧萝'],
    current: 0
  }
  render() {
    return (
      <div>
        <div>女朋友：{this.state.list[this.state.current]}</div>
        <button onClick={this.random}>随机</button>
      </div>
    )
  }
 // 点击随机女朋友 
  random = () => {
    this.setState({
      current: parseInt(Math.random() * this.state.list.length)
    })
  }
  // 状态变化时触发
  shouldComponentUpdate(nextProps, nextState) {
    if (this.state.current === nextState.current) {
      // 状态没有发生改变 return false 就不触发render() 
      return false
    } else {
      // return true 触发render()
      return true
    }
  }
}
// 渲染组件
ReactDOM.render(<App />, document.getElementById('root'))
```

## PureComponent  纯组件  

```react
// 不建议用   用的不好会有隐蔽性的bug

import React, { PureComponent } from 'react'
import ReactDOM from 'react-dom'


// state 和props的值(地址)没有变化 就不会重新渲染
class App extends PureComponent {
  render() {
    return <div></div>
  }
}
// 渲染组件
ReactDOM.render(<App />, document.getElementById('root'))

```



# 路由

## **核心组件**

```react
// 1. 安装依赖包
  yarn add react-router-dom

// 2. 导入核心组件
import { HashRouter as Router, Link, Route,Switch,Redirect } from 'react-router-dom'

// Router
1.  HashRouter或者HashRouter包裹整个应用，一个项目中只会有一个Router
	1.1  HashRouter     使用 URL 的哈希值实现 原理:监听window的hashchange事件
 	1.2  BrowserRouter  使用 H5 的 history API 实现  原理:监听window的popstate事件来实现

// Link
2.1.  Link       指定导航链接替换成a标签  
2.2   NavLink    
`属性`:
to : 要跳转的url
activeClass : 用于指定高亮的类名，默认`active`  // NavLink才有此属性
exact : 精确匹配，表示必须精确匹配类名才生效      // NavLink才有此属性 路径配置了/ ,都需要配置此属性

// Route
3.  Route      指定路由规则   
`属性`:
path : 匹配路径        // 没有指定 path，那么component对应组件一定会被渲染
component : 匹配路径渲染的组件

// Switch
+ 通常，我们会把`Route`包裹在一个`Switch`组件中
+ 在`Switch`组件中，不管有多少个路由规则匹配到了，都只会渲染第一个匹配的组件
+ 通过`Switch`组件非常容易的就能实现404错误页面的提示

// Redirect
`属性`:
from: 要去的路径
to:  需要重定向的路径
exact  : 精确匹配
```



## 路由执行过程

```
1. 浏览器url 发生改变
2. React路由监听到url变化
3. react路由内部遍历所有Route组件, puth 与 pathname(hash) 进行匹配
4. 能够匹配就展示该Route组件的内容
```

## 编程式导航

```react
// history
通过this.props.history 获取浏览器历史记录的相关信息

this.props.history.push(path):跳转到某个页面 参数path表示要跳转的路径

this.props.history.go(n) : 前进或后退到某个页面 参数n表示前进或后退页面数量

// match
<Route exact path="/user/:id" component={Users}>
通过this.props.match.params.id  获取动态路由参数


// to传的是对象
<Redirect to={{pathname:"/login",search="?id=123",state:{from:Props.loaction.pathname } }}  />

通过this.props.loaction 可以拿到search和state
```

#  

# react样式冲突

```js
react样式冲突解决方案

1. 手动处理 （起不同的类名）

2. CSS IN JS ： 以js的方式来处理css
推荐使用：CSS Modules （React脚手架已集成，可直接使用）
// CSS Modules会帮我们自动生成的类名，我们只需要提供 classname 即可
生成类名的格式为  [filename]_[classname]_[hash]

// 1. 在组件中创建的样式文件名称：   假设此文件为.test类名设置了样式
index.module.css

// 2. 在组件用导入js的形式导入样式文件： 
import styles from './index.module.css'

// 3. 通过 styles 对象访问对象中的样式名来设置样式
<div className={styles.test}></div>

// 注意点:
// 1  .user-list的短横线样式  使用 => styles['user-list']  最好用驼峰 userList命名

// 2  因为CSS Modules会把类名给改掉  如果对字体图标 iconfont 设置类名,字体图标不会生效
// :global(类名)  被:global包裹的类名不会被修改 为全局类名
 .aa :global(.iconfont) { color:red }
```



# Router

```react
// 底层Router 

import { createHashRouter } from 'history'

HashRouter = <Router history={createHashRouter()} />
```

# 表达式渲染

```react
            <div
              className="result-value"
              dangerouslySetInnerHTML={{
                __html: highlight(item, keyword),
              }}
            ></div>
```

