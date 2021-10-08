# react包

# 快捷路径

```react
// path.ts.config.json里配置
{
  "compilerOptions": {
    "baseUrl": "./",
    "paths": {
      "@/*": ["src/*"],
      "@scss/*": ["src/assets/styles/*"]
    }
  }
}
```



# 常用包

```
react-router-dom   5
redux
react-redux        7
redux-devtools-extension
redux-thunk
classnames
```

## axios

```react
yarn add axios
import axios from 'axios'

const instance = axios.create({
  timeout: 5000,
  baseURL
})

instance.interceptors.request.use(config=>{},error=>{})

instance.interceptors.response.use(response=>{},error=>{})
```



## antd-mobile

https://mobile.ant.design/docs/react/use-with-create-react-app-cn

```react
// 按需加载
yarn add customize-cra react-app-rewired babel-plugin-import -D

// config-overrides.js 配置 覆盖webpack配置
// package.json里修改
  "scripts": {
    "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test",
    "eject": "react-scripts eject"
  },
```

## antd-mobile-v5

```
 "antd-mobile-v5": "npm:antd-mobile@next",
```



## react-quill

```react
 import ReactQuill from 'react-quill'
 import 'react-quill/dist/quill.snow.css'
 
 
 <ReactQuill theme="snow" placeholder="请输入文章内容..." />
```



## formik+yup

```react
yarn add formik yup

import { useFormik } from 'formik'
import * as Yup from 'yup'


  const formik = useFormik({
    initialValues: {
      mobile: '13911111111',
      code: '246810'
    },
    // 当表单提交的时候，会触发
    async onSubmit(values) {
      await dispatch(login(values))
      Toast.success('登录成功')
      const pathname = location.state ? location.state.from : '/home'
      // history.go(-1) 页面回退
      // history.go(1) 页面前进
      // history.push() 页面跳转，并且往页面栈中添加一条记录
      // history.replace() 页面跳转，但是不会添加一条记录，而且替换当前的记录
      history.replace(pathname)
    },

    validationSchema: Yup.object({
      mobile: Yup.string()
        .required('手机号不能为空')
        .matches(/^1[3-9]\d{9}$/, '手机号格式错误'),
      code: Yup.string()
        .required('验证码不能为空')
        .matches(/^\d{6}$/, '验证码格式错误')
    })
  })
  const {
    values: { mobile, code },
    handleChange,
    handleSubmit,
    handleBlur,
    errors,
    touched,
    isValid
  } = formik
```



## lodash

```
yarn add lodash
```

## sass

```
yarn add sass --save-dev
```

## postcss-px-to-viewport -D

```react
// 用于覆盖webpack的配置
const {
  override,
  fixBabelImports,
  addWebpackAlias,
  addPostcssPlugins,
} = require('customize-cra')
const px2viewport = require('postcss-px-to-viewport')
const path = require('path')

/* 
  css处理器
    预处理器：less sass stylus
    后处理器：postcss (js中的babel)
      autoprefixer : 自动添加前缀  tranform:
      pxtorem :px 转成rem
      pxtoviewport px转成vw
*/
// antd 的按需加载
const babelPlugin = fixBabelImports('import', {
  libraryName: 'antd-mobile',
  style: 'css',
})

// 配置别名
const alias = addWebpackAlias({
  '@': path.join(__dirname, 'src'),
  '@scss': path.join(__dirname, 'src/assets/styles'),
})

const postcssPlugins = addPostcssPlugins([
  px2viewport({
    viewportWidth: 375,
  }),
])

module.exports = override(babelPlugin, alias, postcssPlugins)
```



## amfe-flexible+postcss-pxtorem -D





## socket.io-client

```react
import io from 'socket.io-client'

useEffect(() => {
    dispatch(getUser())
    const client = io('http://geek.itheima.net', {
      query: {
        token: getTokenInfo().token
      },
      transports: ['websocket']
    })
    clientRef.current = client
    // 连接成功事件
    client.on('connect', function () {
      setMessageList(messageList => {
        return [
          ...messageList,
          { type: 'robot', text: '我是小智,有什么想要问我的' }
        ]
      })
    })
    // 接收服务器返回的消息
    client.on('message', function (e) {
      setMessageList(messageList => {
        return [...messageList, { type: 'robot', text: e.msg }]
      })
    })
    return () => {
      // 组件销毁  关闭连接
      client.close()
    }
  }, [])
  // 聊天记录定位最底部
  useEffect(() => {
    listRef.current.scrollTop =
      listRef.current.scrollHeight - listRef.current.offsetHeight
  }, [messageList])
```

## typescript

```
yarn add typescript
```



## dompurify

```react
npm install dompurify
// 包裹文本   解决xss
DOMPurify.sanitize(dirty)  //DOMPurify.sanitize('<img src=x onerror=alert(1)//>')
```

## highlight.js

```react
import hljs from 'highlight.js'
// highlightElement(DOM) 就可以让代码高亮
import 'highlight.js/styles/monokai.css'

// 导入高亮的语言包
import 'highlight.js/lib/common'

// 使用
// 配置 highlight.js
hljs.configure({
  // 忽略未经转义的 HTML 字符
  ignoreUnescapedHTML: true
})
// 获取到内容中所有的code标签
const codes = document.querySelectorAll('.dg-html pre > code')
codes.forEach(el => {
  // 让code进行高亮
  hljs.highlightElement(el as HTMLElement)
})
```



## dayjs

```react
import dayjs from 'dayjs'
// 扩展dayjs，有显示相对时间的功能
import relativeTime from 'dayjs/plugin/relativeTime'
// 导入中文包
import 'dayjs/locale/zh-cn'
dayjs.extend(relativeTime)
dayjs.locale('zh-cn')


// 使用
dayjs(时间).format('YYYY-MM-DD')
```

## react-virtualized

```
// 1. yarn add react-virtualized


// 2. import 'react-virtualized/styles.css'


// 3. 使用list组件渲染长列表
AutoSizer高阶组件让list撑满屏幕
https://github.com/bvaughn/react-virtualized/blob/master/docs/AutoSizer.md

// 右侧ul 用index遍历出来  给li绑定点击事件 进行跳转功能

// 跳转到对应index
   cityListComponent.current.scrollToRow(index)

// 组件ref实例方法 提前计算每一行的高度 ,实现精确跳转
 在有数据后调用  不然会报错
cityListComponent.current.measureAllRows()
```



```react
import { Toast } from 'antd-mobile'
import './index.scss'
import { useEffect, useState, useRef } from 'react'
// 接口
import { getCitysList, getCitysHot } from '../../api/index.js'
// 获取定位的方法
import { getCurrentCity } from '../../utils'
// 长列表组件
import { List, AutoSizer } from 'react-virtualized'
// NavHeader组件
import NavHeader from '../../components/NavHeader'
// 数据格式化的方法
const formatCityData = list => {
  const cityList = {}
  list.forEach(item => {
    const first = item.short.substr(0, 1)
    if (cityList[first]) {
      cityList[first].push(item)
    } else {
      cityList[first] = [item]
    }
  })
  const cityIndex = Object.keys(cityList).sort()
  return {
    cityList,
    cityIndex
  }
}
// 处理字母索引的方法
const formatCityIndex = letter => {
  switch (letter) {
    case '#':
      return '当前定位'
    case 'hot':
      return '热门城市'
    default:
      return letter.toUpperCase()
  }
}

// 高度
const TITLE_HEIGHT = 36
const NAME_HEIGHT = 50
// 长列表
// const list = Array(100).fill('Brian Vaughn')
// 渲染每一行数据的渲染函数  返回值表示最终渲染在页面中的内容
// function rowRenderer({
//   key, // Unique key within array of rows
//   index, // 索引
//   isScrolling, // 当前项是否正在滚动  布尔值
//   isVisible, // 当前项在list中是可见的
//   style // 重点属性,一定要给每一个行数据添加改样式  作用:指定每一行的位置
// }) {
//   return (
//     <div key={key} style={style}>
//       {list[index]}
//     </div>
//   )
// }
const HOUSE_CITY = ['北京', '上海', '广州', '深圳']
const CityList = ({ history }) => {
  const cityListComponent = useRef(null)
  const [cityList, setCityList] = useState({})
  const [cityIndex, setCityIndex] = useState([])
  const [activeIndex, setActiveIndex] = useState(0) // 当前index 用于添加类名

  useEffect(() => {
    ;(async () => {
      // 获取城市列表
      await getcityList()
      // 组件ref实例方法 提前计算每一行的高度 ,实现精确跳转
      cityListComponent.current.measureAllRows()
    })()
  }, [])
  // 获取城市列表
  async function getcityList() {
    // 全部城市列表
    const res = await getCitysList()
    const { cityList, cityIndex } = formatCityData(res.data.body)
    // 添加热门城市
    const hot = await getCitysHot()
    cityList['hot'] = hot.data.body
    cityIndex.unshift('hot')
    // 获取当前定位城市
    const curCity = await getCurrentCity()
    cityList['#'] = [curCity]
    cityIndex.unshift('#')
    console.log(cityList, cityIndex, curCity)
    setCityList(cityList)
    setCityIndex(cityIndex)
  }

  // 改变城市
  const changeCity = ({ label, value }) => {
    if (HOUSE_CITY.indexOf(label) > -1) {
      // 有房源则保存到本地
      localStorage.setItem('hkzf_city', JSON.stringify({ label, value }))
      history.go(-1)
    } else {
      Toast.info('该城市暂无房源数据', 1, null, false)
    }
  }
  // 渲染每一行数据的渲染函数  返回值表示最终渲染在页面中的内容
  const rowRenderer = ({
    key, // Unique key within array of rows
    index, // 索引
    isScrolling, // 当前项是否正在滚动  布尔值
    isVisible, // 当前项在list中是可见的
    style // 重点属性,一定要给每一个行数据添加改样式  作用:指定每一行的位置
  }) => {
    // latter
    const latter = cityIndex[index]
    return (
      <div key={key} style={style} className="city">
        <div className="title">{formatCityIndex(latter)}</div>
        {cityList[latter].map(item => (
          <div
            className="name"
            key={item.value}
            onClick={() => changeCity(item)}
          >
            {item.label}
          </div>
        ))}
      </div>
    )
  }
  // 动态计算高度
  function getRowHeight({ index }) {
    return TITLE_HEIGHT + cityList[cityIndex[index]].length * NAME_HEIGHT
  }
  // 封装渲染右侧索引列表的方法
  function renderCityIndex() {
    // 获取到 cityIndex，并遍历其，实现渲染
    return cityIndex.map((item, index) => (
      <li
        className="city-index-item"
        key={item}
        onClick={() => {
          // console.log('当前索引号：', index)
          // 跳转到对应index
          cityListComponent.current.scrollToRow(index)
        }}
      >
        <span className={activeIndex === index ? 'index-active' : ''}>
          {item === 'hot' ? '热' : item.toUpperCase()}
        </span>
      </li>
    ))
  }
  // 获取List组件渲染行的信息
  function onRowsRendered({ startIndex }) {
    if (activeIndex !== startIndex) {
      setActiveIndex(startIndex)
    }
  }
  return (
    <div className="citylist">
      {/* 顶部导航 */}
      <NavHeader>城市选择</NavHeader>
      {/* 城市列表 */}
      {/* 
    rowCount  行长度
    rowHeight  行高度
    rowRenderer 返回值代表每一行的ui
    onRowsRendered  每行的信息
    scrollToAlignment start 滚到顶部
    */}
      <AutoSizer>
        {({ height, width }) => (
          <List
            ref={cityListComponent}
            width={width}
            height={height}
            rowCount={cityIndex.length}
            rowHeight={getRowHeight}
            rowRenderer={rowRenderer}
            onRowsRendered={onRowsRendered}
            scrollToAlignment="start"
          />
        )}
      </AutoSizer>
      {/* 右侧索引列表 */}
      <ul className="city-index">{renderCityIndex()}</ul>
    </div>
  )
}

export default CityList
```

## 百度地图API

```
// 地图找房
百度地图API
1.  服务介绍=>  逆/地址解析
2.  服务介绍=>  添加控件
3. 示例DEMO =>  点覆盖物 => 自定义文本标注
4. 类参考 => 覆盖物类 Label  => setContent() 设置文本标注的内容。支持HTML  setStyle  设置文本标注样式，该样式将作用于文本标注的容器元素上
5. label 原生绑定点击事件


// react-virtualized
概述: react组件  用于高效渲染大型列表和表格数据
```



```react
import './index.scss'
import { useEffect } from 'react'
import { getMap } from '../../api/index.js'
import NavHeader from '../../components/NavHeader'
// import './index.scss'
import styles from './index.module.scss'
const BMap = window.BMap
// 覆盖物样式
const labelStyle = {
  cursor: 'pointer',
  border: '0px solid rgb(255, 0, 0)',
  padding: '0px',
  whiteSpace: 'nowrap',
  fontSize: '12px',
  color: 'rgb(255, 255, 255)',
  textAlign: 'center'
}
const Map = () => {
  useEffect(() => {
    initMap()
  }, [])
  // 初始化地图
  const initMap = () => {
    // 获取当前定位城市
    const { label, value } = JSON.parse(localStorage.getItem('hkzf_city'))
    console.log(label, value)

    // 注意: 在脚手架中全局对象需要使用window来访问
    // 初始化地图实例
    const map = new BMap.Map('container')
    // 设置中心点坐标
    // const point = new BMap.Point(116.404, 39.915)

    // 创建地址解析器实例
    const myGeo = new BMap.Geocoder()
    // 将地址解析结果显示在地图上，并调整地图视野
    myGeo.getPoint(
      label,
      async function (point) {
        if (point) {
          // 初始化地图
          map.centerAndZoom(point, 11) // 缩放级别11
          // map.addOverlay(new BMap.Marker(point))  //标记物

          // 添加常用控件
          map.addControl(new BMap.NavigationControl()) // 缩放
          map.addControl(new BMap.ScaleControl()) // 比例尺

          // 获取房源数据
          const res = await getMap(value)
          console.log('房源数据', res)

          // 遍历房源 创建覆盖物
          res.data.body.forEach(item => {
            // 解构出区的地理位置
            const {
              coord: { longitude, latitude },
              label: areaName,
              count,
              value
            } = item
            // 创建覆盖物文本标注对象
            const areaPoint = new BMap.Point(longitude, latitude)
            const opts = {
              position: areaPoint, // 指定文本标注所在的地理位置
              offset: new BMap.Size(-35, -35) // 设置文本偏移量
            }
            // 设置setContent后 第一个参数失效  清空即可
            const label = new BMap.Label('', opts)

            // 添加唯一标识
            label.id = value
            // 设置房源覆盖物内容
            label.setContent(`
          <div class="${styles.bubble}">
            <p class="${styles.name}">${areaName}</p>
            <p>${count}</p>
          </div>
        `)

            // 自定义文本标注样式
            label.setStyle(labelStyle)

            // 添加单击事件
            label.addEventListener('click', () => {
              console.log(label.id)

              // 放大地图, 当前点击的覆盖物为中心  11=13
              map.centerAndZoom(areaPoint, 13)

              // 加定时器 解决百度API报错问题
              setTimeout(() => {
                // 清除当前覆盖物
                map.clearOverlays()
              }, 0)
            })

            // 添加覆盖物到地图中
            map.addOverlay(label)
          })
        }
      },
      label
    )
    // 初始化地图
    // map.centerAndZoom(point, 15)
  }
  return (
    <div className={styles.map}>
      {/* 顶部导航 */}
      <NavHeader>地图找房</NavHeader>
      <div id="container" className="container">
        111
      </div>
    </div>
  )
}

export default Map

```

