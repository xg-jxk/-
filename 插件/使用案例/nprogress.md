# nprogress

```js
// 入口文件引入 进度条js 和css
import NProgress from 'nprogress'
import 'nprogress/nprogress.css'

// 拦截器配置
// 请求拦截器 
axios.interceptors.request.use(config => {
  NProgress.start()
  return config
})
// 响应拦截器
axios.interceptors.response.use(config => {
  NProgress.done()
  return config
})
```

