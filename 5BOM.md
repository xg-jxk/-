# BOM

## BOM简介概述

BOM（Brower Object Model) 浏览器对象模型，提供了独立于内容而与浏览器窗口进行交互的对象，核心对象是window一系列的相关构成对象，每一个都提供了很多方法和属性；

BOM缺乏标准，javascript语法标准化组织是ECMA，DOM的标准化组织是W3C,BOM最初是Netscaoe浏览器标准的一部分；

window对象是浏览器的顶级对象，具有双重角色，是JS访问浏览器窗口的一个接口，是一个全局对象，定义在全局作用域中的变量、函数都会变成window对象的属性和方法。在调用的时候可以省略window，之前学习的对话框都属于window对象方法，如alert()、prompt()等；

**注意：window下的一个特殊属性window.name，所以我们在定义变量名称时尽量避免用name；**



## DOM和BOM的区别

BOM比DOM更大，包含了DOM

DOM文档对象模型，是把文档当做一个对象来看待，DOM的顶级对象是document，DOM主要学习的是操作页面元素，是W3C标准规范；

BOM浏览器对象模型，是把浏览器当做一个对象来看待，BOM的顶级对象是window，BOM是学习浏览器窗口交互的一些对象，是浏览器厂商在各自浏览器上定义的，兼容性较差；



# **URL**

| ***\*组成\**** | ***\*说明\****                                               |
| -------------- | ------------------------------------------------------------ |
| protocol       | 通信协议，常用http，ftp，maito等                             |
| host           | 主机（yuming）[www.itcast.cn](www.itcast.cn)                 |
| port           | 端口号可选，省略时使用方案的默认端口，如http的默认端口为80   |
| path           | 路径由零个或者多个'/'符号隔开的字符串，一般用来表示主机上的一个目录或者文件地址 |
| query          | 参数，以键值对的形式通过&符号分隔开来                        |
| fragment       | 片段，#后面内容常见于链接锚点                                |



# location对象

## location属性

| location对象属性     | 返回值                              |
| :------------------- | ----------------------------------- |
| **location.href⭐**   | **获取或者设置整个URL**             |
| location.host        | 返回主机（域名） www.itheima.cn     |
| location.port        | 返回端口号，如果未写返回空字符串    |
| location.pathname    | 返回路径                            |
| **location.search⭐** | **返回参数**                        |
| location.hash        | 返回片段 #后面内容  常见于链接 锚点 |

## location方法

| location对象方法       | 返回值                                                       |
| ---------------------- | ------------------------------------------------------------ |
| location.assign()      | 跟href一样，可以跳转页面（也称为重定向页面）                 |
| location.replace()     | 替换当前页面，因为不记录历史，所以不能后退页面               |
| **location.reload()⭐** | 重新加载页面，相当于刷新按钮或者F5，如果参数为true是强制刷新Ctrl+F5 |





# navigator对象(了解)

navigator对象包含有关浏览器的信息，它有很多属性，我们最常用的是userAgent,该属性可以返回由客户机发送服务器的user-agent头部的值；

下面前端代码可以判断用户在哪个终端打开页面，实现跳转

```js
<script>
    if ((navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i))) {
        window.location.href = ""; //手机端网址
    } else {
        window.location.href = ""; //PC端网址
    }
</script>
```

注意：实际工作中判断页面在现实终端都是后台工作人员去实现的

## 获取地理位置信息

```js
// 获取地理位置信息
// position对象表示当前位置信息
// latitude 维度 / longitude 经度
// accuracy 经纬度的精度 / altitude 海拔高度 / altitudeAccuracy 海拔高度的精度 / heading 设备行走方向 / speed速度
navigator.geolocation.getCurrentPosition(position => {
  debugger
  console.log(1)
  console.log('当前位置信息：', position)
})
```



# history对象(了解)

window对象给我们提供了一个history对象，与浏览器历史记录进行交互，该对象包含用户（在浏览器窗口中）访问过的URL；

| history对象方法 | 作用                                              |
| --------------- | ------------------------------------------------- |
| back()          | 可以后退功能                                      |
| forward()       | 前进功能                                          |
| go(参数)        | 前进后退功能，参数如果是1前进1个页面，后退1个页面 |



# 本地存储⭐

随着互联网的快速发展，基于网页的应用越来越普遍，同时也变的越来越复杂，为了满足各种各样的需求，会经常性在本地存储大量的数据，HTML5规范提出了相关的解决方案；

本地存储的特性：

01、数据存储在用户浏览器中

02、设置、读取方便、甚至页面刷新不丢失数据；

03、容量较大，sessionStorage约5M，localStorage约20M；

04、只能存储字符串，可以将对象JSON.stringify()编码后存储；

## window.sessionStorage

01、生命周期是关闭浏览器窗口；

02、在同一个窗口（页面）下数据可以共享；

03、以键值对的形式存储使用；

## window.localStorage

1、声明周期永久生效，除非手动删除 否则关闭页面也会存在

2、可以多窗口（页面）共享（同一浏览器可以共享）

3、以键值对的形式存储使用

```
setItem('uname', val)
getItem('uname')
removeItem('uname')
clear() 
```





# 定时器⭐

注意：

01、window在调用的时候可以省略；

02、调用函数可以**直接写函数**或者**写函数名**或者采取字符串‘函数名()’三种形式，第三种不推荐使用；

03、延迟的毫秒数可以省略默认是0，如果不省略单位是毫秒；

04、因为定时器可能有很多，我们在使用的时候给定时器赋值一个标识符,也就是起一个名字；

05、setTimeout()只能执行一次,setInterval() 定时执行代码

## setTimeout()  

语法：window.setTimeout(调用函数,[延迟的毫秒数])；

## setInterval()

语法：setInterval(回调函数,[间隔时间])

```js
        function tim() {
            alert('我是老王');
        }
        // 直接调用函数名
        setTimeout(tim, 2000);
        // 直接调用函数
        setTimeout(function () {
            alert('我是老王222');
        }, 1000);
        // 取名字区分不同的定时器效果
        var timer1 = setTimeout(tim, 6000);
        var timer2 = setTimeout(tim, 3000);
		//关闭定时器
		clearTimeout(timer1)
		clearInterval()
```



