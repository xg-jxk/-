# postcss-pxtorem

## 网易云配置

```js
// postcss.config.js配置
module.exports = {
    plugins: {
      'postcss-pxtorem': {
        // 能够把所有元素的px单位转成Rem
        // rootValue: 转换px的基准值。
        // 例如一个元素宽是75px，则换成rem之后就是2rem。
        rootValue: 37.5,
        propList: ['*']
      }
    }
  }
```

## 头条配置

```js
// postcss.config.js配置
module.exports = {
  plugins: {
    // 配置使用 autoprefixer 插件  作用：生成浏览器 CSS 样式规则前缀
    // VueCLI 内部已经配置了 autoprefixer 插件 无需配置,否则启动项目会冲突
    // autoprefixer: {
    //   browsers: ['Android >= 4.0', 'iOS >= 8']
    // },
    // 配置使用 postcss-pxtorem 插件 作用：把 px 转为 rem
                   
    'postcss-pxtorem': {
      rootValue ({ file }) { // file为处理的文件路径  表示根元素字体大小，它会根据根元素大小进行单位转换
        return file.indexOf('vant') !== -1 ? 37.5 : 75
      },
      propList: ['*'] // 用来设定可以从 px 转为 rem 的属性 例如 *就是所有属性都要转换，`width` 就是仅转换 `width` 属性
    }
  }
}
```

