# 组件复用的技术

## mixins(已废弃)

```
缺点:
 1. 数据来源不明确,不容易维护
 2. 方法名冲突
 3. 滚雪球般的复杂性
```



## render-props

```react
基本使用: 
 1. 将公用的状态和方法封装到一个组件中  
 2. 在组件标签中通过props传递一个回调函数  函数的返回值就是需要渲染的ui ,回调函数的参数就是组件提供的状态
 3. 组件的render中调用父组件的props,通常将这个prop命名为render,状态以render参数的形式传递出去
 
  render(){ return this.props.render(this.state)}调用父组件的render属性
```

```react

class Mouse extends React.Component {
	// … 省略state和操作state的方法
    render() {
    	return this.props.render(this.state)
    }
}

<Mouse render={(mouse) => (
	<p>鼠标当前位置 {mouse.x}，{mouse.y}</p>
)}/>

//--------------------children代替props------------------------

class Mouse extends React.Component {
	// … 省略state和操作state的方法
    render() {
    	return this.props.children(this.state)
    }
}

<Mouse> 
 {(mouse) => (
	<p>鼠标当前位置 {mouse.x}，{mouse.y}</p>
)}
<Mouse/>
```



## HOC

```
一个函数，接收要包装的组件，返回增强后的组件

使用规则
 1. 函数名称约定以with开头
 2. 参数(传入的组件)只渲染基本的UI
 3. 函数内部创建一个类组件,提供状态和逻辑,并将这个类组件return
```

```react
// 高阶组件内部创建的类组件：
const WithMouse = (Base) => {
  class Mouse extends React.Component {
        // 处理鼠标的位置等操作
        render() {
            return <Base {...this.state} {...this.props}  />
        }
	}	
  return Mouse
}

// 创建组件
const MousePosition = withMouse(Position)

// 渲染组件
<MousePosition />
```



# Hooks

```js
开发模式
16.8之后  Hooks (提供状态)  + 函数组件 (展示内容)

以下问题 函数组件+hooks都可以解决
类组件存在的问题  
 1. 函数组件和类组件的选择问题
 2. 要关注注册事件回调函数的this指向问题以及class的this怎么工作的
 3. 代码要拆分到不同的生命周期钩子中,不便于维护
 4. 相比于函数组件,不利于代码压缩和优化(tree shaking 树摇),也不利于TS的类型推导

render-props和HOC存在的问题
JSX嵌套地狱问题
重新组织组件结构

`hooks优势`
 1. 复用组件状态逻辑,无需改变组件层次结构
 2. 根据功能而不是基于生命周期进行代码分割
 3. 更好的类型推导
 4. tree--shaking 友好,打包会去掉未引用的代码
 5. 更好的压缩
```

```
tree shaking 树摇   (类组件使用不了)
代码优化 把没用到的方法和变量剔除掉
```



## useState

```react
// useState不能在if/for/swich/函数中使用, 从开发者工具可以看出 useState在Hook里是按顺序查找的
// useState初始值如果要通过复杂的计算获取,可以使用回调函数 return 的形式,只在组件创建加载
import ReactDOM from 'react-dom'
import { useState } from 'react'
function App() {
  
  // 参数 :初始值
  // 返回值: 数组 下标0代表状态 下标1代表修改这个状态的函数
  const [count, setCount] = useState(0)
  return (
    <div>
      <div>{count}</div>
      <button onClick={() => setCount(count + 1)}>+1</button>
    </div>
  )
}

ReactDOM.render(<App />, document.getElementById('root'))
```



## useEffect

```react
// 渲染和更新都会触发回调 相当于componentDidMount + 相当于componentDidUpdate
// 处理函数组件除渲染之外的副作用   例如 Ajax , dom操作
// useEffect本身不能使用async修饰 因为返回值是一个函数   async返回的是promise

减少不必要的diff  组件销毁后 就不要执行修改状态的操作了 // 出现场景 在请求未完成切换组件
useEffect中定义变量a控制组件的状态   let a = false 
在 修改状态上面 if (a) return
useEffect的return里  a = true
```



```react
import ReactDOM from 'react-dom'
import { useState, useEffect } from 'react'
function App() {
  // 参数 :初始值
  // 返回值: 数组 下标0代表状态 下标1代表修改这个状态的函数
  const [count, setCount] = useState(0)
  const [money, setMoney] = useState(100)
  // 渲染和更新都会触发回调 相当于componentDidMount + 相当于componentDidUpdate
  // 参数1: 回调函数
  // 参数2: 空数组,代表只有初次渲染时触发, 定时器,Ajax
  // 参数2: 数组指定依赖项,只会初次渲染和依赖状态值改变时触发
  // 回调里用到了依赖 就在参数2中添加此依赖
  // 参数1的回调中return一个函数称为清理副作用的函数 此函数在组件销毁的时候执行
  useEffect(() => {
    document.title = `当前点击了${count}次`
    console.log('触发了')
  }, [count])
  return (
    <div>
      <div>{count}</div>
      <button onClick={() => setCount(count + 1)}>打豆豆</button>
      <div>{money}</div>
      <button onClick={() => setMoney(money + 100)}>+100</button>
      {count < 5 ? <Child /> : <div>豆豆死了</div>}
    </div>
  )
}
// 
function Child() {
  useEffect(() => {
    const timer = setInterval(() => {
      console.log('打印了')
    }, 1000)
    return () => {
      clearInterval(timer)
    }
  }, [])
  return <div>我是子组件</div>
}

ReactDOM.render(<App />, document.getElementById('root'))
```

## useRef

```react
import { useRef } from 'react'

const TodoAdd = ({ addItem }) => {
  const input = useRef(null)
  const onKeyUp = e => {
    if (e.keyCode === 13) {
      addItem(input.current.value)
      e.target.value = ''
    }
  }
  return (
    <header className="header">
      <h1>todos</h1>
      <input
        ref={input}
        className="new-todo"
        placeholder="What needs to be done?"
        autoFocus
        onKeyUp={onKeyUp}
      />
    </header>
  )
}
export default TodoAdd
```



## useContext

```react
// 数据提供方
export const Context = React.createContext()
const App = () => {
  const [color, setColor] = useState('red')
  return (
    <Context.Provider value={color}>
		....
    </Context.Provider>
  )
}

// 数据接收方
import { Context } from '../App.js'
import { useContext } from 'react'
const Child = () => {
  const color = useContext(Context)
  return (
    <div>
      <h5>我是子组件--{color}</h5>
    </div>
  )
}
```



## React.memo()

```react
// 相当于类组件的PureComponent  或者shouldComponentUpdate


import React, { useState, memo } from 'react'

const App = () => {
  const [count, setCount] = useState(0)
  return (
    <div>
      <h1>根组件--{count}</h1>
      <button onClick={() => setCount(count + 1)}>点击</button>
      <Child1 count={count}></Child1>
      <Child2></Child2>
    </div>
  )
}

const Child1 = memo(({ count }) => {
  console.log('child1更新')
  return <div>Child1组件{count}</div>
})

const Child2 = memo(() => {
  console.log('child2更新')
  return <div>Child2组件</div>
})

export default App
```



## useCallback

```
缓存函数  
```



```react
import React, { memo } from 'react'

import { useState, useCallback } from 'react'
function ParentComp() {
  const [count, setCount] = useState(0)
  // 父组件渲染时会创建一个新的函数
  const increment = () => setCount(count + 1)

  const [name, setName] = useState('hi~')
  // const changeName = newName => setName(newName)
  const changeName = useCallback(newName => setName(newName), [])
  return (
    <div>
      <button onClick={increment}>点击次数：{count}</button>
      <ChildComp name={name} onClick={changeName} />
    </div>
  )
}

const ChildComp = memo(function ({ name, onClick }) {
  console.log('子组件')
  return (
    <>
      <div>子{name}</div>
      <button onClick={() => onClick('hello')}>改变nam 值</button>
    </>
  )
})

export default ParentComp
```



## useMemo

```
缓存任意类型的数据

如果是一个昂贵的计算, 为了避免每次状态改变导致每次渲染执行 就可以使用useMemo

只在依赖状态改变的时候在执行  类似于vue的计算属性
```

```react
import React, { useState, useMemo } from 'react'

const App = () => {
  const [money, setMoney] = useState(100000)
  const total = useMemo(() => {
    return Array.from(new Array(money))
      .map((item, i) => i + 1)
      .reduce((pre, item) => pre + item, 0)
  }, [money])

  return (
    <div>
      <h1>{total}</h1>
      <button onClick={() => setMoney(money + 100)}>+100</button>
    </div>
  )
}

export default App
```



## useReducer

```
一般配合useContext使用  无需将大量的属性传给组件 只需要把dispatch传给组件  
在子组件中使用dispatch(action) 来修改状态

reducer 和action抽离到单独文件  
```

# 自定义hook

```react
// 使用hooks实现猫跟着鼠标移动
import { useEffect, useState } from 'react'
export default function useMouse() {
  const [position, setPosition] = useState({
    x: 0,
    y: 0,
  })

  useEffect(() => {
    const move = (e) => {
      setPosition({
        x: e.pageX,
        y: e.pageY,
      })
    }
    document.addEventListener('mousemove', move)
    return () => {
      document.removeEventListener('mousemove', move)
    }
  }, [])
  return position
}

```



```react
const initialState = {count: 0};

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
```



# JSON-server

```react
// 模拟REST规范的接口   get post put patch delete      http://.../user/1
// 1. 准备一个json文件  文件中提供数据  data.json

// 2. 全局安装  yarn global add json-server

// 3. 输入命令生成接口地址  端口号8888
json-server data.json --port 8888
```



# immer

```
直接修改数据
```



# 非路由组件获取history



```
通过导入 react-router-dom的withRouter 高阶组件包裹   或者通过 const history=useHistory()
```

