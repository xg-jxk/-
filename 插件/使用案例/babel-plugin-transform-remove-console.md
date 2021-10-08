# babel-plugin-transform-remove-console

```js
// 在babel.config.js文件中添加以下代码   
const prodPlugins = []
if(process.env.NODE_ENV === 'production') {
    prodPlugins.push('transform-remove-console')
}
module.exports = {
    plugins: [
        ...... 省略若干,
        ...prodPlugins
    ]
}
```

