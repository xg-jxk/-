# DOM

## DOM简介概述

**DOM**：文档对象模型（Document ObjectModel）,是**W3c组织推荐的处理可扩展标记语言**（html或者XML）的标准程序接口；W3c已经定义了一系列DOM接口，通过这些DOM接口可以改变网页的内容、结构和样式；

**DOM树**：是由文档，元素，节点组成的，DOM中把文档，元素，节点都称之为对象；

**文档：**一个页面就是一个文档，DOM中使用document表示文档；

**元素：**页面中所有的标签都是元素，DOM中使用element表示元素；

**节点：**网页中的所有内容都是节点（标签，属性，文本，注释等），DOM中使用node表示；



## API 和 Web API解释

```javascript
API: 应用程序编程接口，预先定义好了函数，提供应用程序与开发人员基于某软件或者硬件得以访问一组例程的能力，而又无需访问源码，或理解内部工作机制的细节。

简单理解：API是给程序员提供的一种工具，能更轻松的实现想要完成的功能。

Web API：浏览器提供的一套操作浏览器功能和页面元素的API（DOM和BOM）；

现阶段我们主要针对浏览器讲解常用的API，主要做针对浏览器做交互效果；Web API一般都有输入和输出（函数的传参和返回值），Web API 很多都是方法函数；
```





## DOM事件流⭐

```js
//理论

DOM事件流：页面中接收事件的顺序，事件发生时会在元素节点之间按照特定的顺序传播，这个传播过程就是DOM事件流；

DOM事件流分为3个阶段：

01、捕获阶段   ： 添加了事件以后就会一层一层的捕获目标盒子（从外往里传播到当前目标阶段）

02、当前目标阶段 

03、冒泡阶段 ： 找到目标后再从里到外一层一层的传播；

# 注意：（特重要）

01、JS代码中只能执行捕获或者冒泡其中的一个阶段；

02、onclick和attachEvent()ie浏览器，只能得到冒泡阶段；

03、捕获阶段，如果addEventListener 第三个参数是true，表示在事件捕获阶段调用事件处理程序，如果是false(不写默认就是false)，表示时间冒泡阶段调用事件处理程序；

04、实际开发中我们很少使用事件捕获，我们更多的是使用事件冒泡；

05、有些事件是没有冒泡的，比如onblur、onfocus、onmouseenter、onmouseleave;
```



## 获取元素

```js
// 根据ID获取  getElementById
document.getElementById("time");
// 根据标签名获取 getElementsByTagName
document.getElementsByTagName("li");

//H5新增的获取元素的方法
// 根据类名获取 getElementsByClassName
document.getElementsByClassName("lis");
// 根据指定选择器获取第一个元素 querySelector
document.querySelector(".lis");
// 根据指定选择器获取所有querySelectorAll
document.querySelectorAll(".lis");

//特殊标签元素的获取
var bodyEle = document.body;
var htmlEle = document.documentElement;
```

## 元素属性操作

- **元素属性**

```js
//修改元素属性
bYt.onclick = function () {
  oImg.src = "img/01.jpg";
  oImg.title = "我是八姨太";
};
//修改元素样式
oP.onclick = function () {
  this.style.backgroundColor = "red";
  this.style.fontSize = "30px";
  this.style.color = "#fff";
};
//修改元素类名
oP.onclick = function() {
     this.className = 'lw';
}
```

- **自定义属性**

```js
  //获取元素属性
  element.getAttribute('属性')
  //设置修改元素的属性值
  element.setAttribute('属性名','属性值') //主要针对于自定义属性
  //移除元素属性
  element.removeAttribute('属性')
	//  H5新增获取自定义属性的方法 (只能获取data开头)
	element.dataset.属性
```

- **类名**

```js
    var box = document.querySelector(".box");
    box.addEventListener("click", function () {
      this.classList.add("box1"); //添加类名
      this.classList.remove("box"); //移除类名
      box.classList.toggle("show"); //切换类名 (没有就加 有就移除)
    });
```



## 元素节点操作⭐

```javascript
  // 父元素节点
	this.parentNode.className = 'box'
	
	//获取node所有的子元素节点
  var oLi = oUl.children;
  //node的下一个兄弟元素节点
  var oLi = oUl.nextElementSibling;
  //node的上一个兄弟元素节点
  var oLi = oUl.previousElementSibling;

  // 创建节点
  var oLi = document.createElement("li");

	// 添加节点
  //将oLi添加到oUl的子节点末尾（最后面）
  oUl.appendChild(oLi);
  //给oUl中的oUl.children[0]前面添加oLi
  oUl.insertBefore(oLi, oUl.children[0]);
	
  //删除节点
  oUl.removeChild(oUl.children[0]);
  oUl.remove()//删除所有子节点


  //小括号中参数为true，称为深拷贝，将节点本身以及节点内部的子节点全部克隆
  var oLli = oLi[0].cloneNode(true);
```





## 三种动态创建元素的区别

```
document.write()  直接将内容写入到页面的内容流，但是文档流执行完毕，会导致页面全部重绘；

element.innerHTML，效率高于createElement（不建议使用字符串的拼接，建议使用数组）

document.createElement()  结构清晰
```





# 事件三要素

- 事件源 (获取操作元素) 

- 事件类型(绑定事件) 

- 事件处理程序(函数赋值)



## 注册事件/解绑事件

- **方法监听**

```
div.addEventListener('click',fn) 
div.removeEventListener('click',fn)
```

- **传统注册**

```js
// 缺点 : 同一个元素同一个事件只能设置一个处理函数，如果一个按钮同时注册了多个事件，最后注册的会将前面注册的覆盖了
div.onClick = function(){
	.....代码执行  	// 执行完进行解绑事件
	div.onclick = null;
}
```



## 阻止默认行为/阻止冒泡⭐

```javascript
//注意:return false 也能阻止默认行为，没有兼容性问题，但是return后面的代码就不执行了
e.preventDefault()   
//阻止冒泡
e.stopPropagation()
```



# 常用事件⭐

```javascript
//鼠标事件
click            鼠标单击
mouseover        鼠标经过
mouseout         鼠标离开
focus            获取鼠标焦点触发
blur             失去鼠标焦点触发
mousemove        鼠标移入（移动）
mouseup          鼠标弹起触发
mousedown        鼠标按下
scroll           元素滚动
//键盘事件
/**
1、onkeydown  和 onkeyup 不区分字母大小写，如果用onkeypress就需要区分字母大小写；
2、实际开发中，我们更多的使用keydown和keyup，他们能识别所有的键（包括功能键）；
3、keypress不识别功能键，但是keyCode属性能区分大小写，返回不同的ASCLL值 **/
keydown          按下按键时触发
keypress         按下按键时触发 ，对功能键不起作用，比如ctrl、shift、上下左右箭头等 区分字母大小写
keyup            松开按键时触发

//没有冒泡
focus            获取鼠标焦点触发
blur             失去鼠标焦点触发
mouseenter       鼠标移入
mouseleave       鼠标移除

// 移动端
touchstart       触摸触发
touchmove        滑动触发
touchend         离开触发

//e.touches        正在触摸屏幕的所有手指的一个列表               
//e.targetTouches  正在触摸当前DOM元素上的手指的一个列表           
//e.changedTouches 手指状态发生了改变的列表，从无到有，从有到无变化

// 表单拖拽
drag &drop 拖拽

// 文档加载完触发
document.addEventListener('DOMContentLoaded', function (e) {})
window.onload =function(){} 
window.addEventListener('load',function(){})

// 窗口大小改变
window.addEventListener('resize',function(){});

// img
load    // 加载中
error   // 加载失败
```



## 鼠标事件对象 mouseEvent⭐

```js
可视区域client 文档区域page 电脑屏幕 screen

			// 鼠标事件对象 mouseEvent
        document.addEventListener('click', function (e) {
            // client 鼠标在可是区域的X和Y坐标
            console.log(e.clientX);
            console.log(e.clientY);
            // page 鼠标在页面文档的X和Y坐标
            console.log(e.pageX);
            console.log(e.pageY);
            // screen 鼠标在电脑屏幕的X和Y坐标
            console.log(e.screenX);
            console.log(e.screenY);
        })
```





## 元素偏移量offset

```js
son.offsetParent  //返回带有定位的父级元素 没有则返回body
box.offsetLeft    //返回离带有定位父级盒子左边的距离 没有定位则已body为准 
box.offsetTop
son.offsetHeight //可以得到元素的大小 宽度和高度 是包含padding + border + width
son.offsetWidth
```

## 元素可视区 client

```js
div.clientTop     // element.clientTop返回元素上边框的大小
div.clientLeft    // element.clientLeft返回元素左边边框的大小
div.clientWidth   // element.clientWidth返回自身包括padding。内容的宽度，不含边框，返回数值不带单位
div.clientHeight  // element.clientHeight返回自身包括padding、内容区的高度，不含边框，返回数值不带单位
```

## 元素滚动scroll

```js
oDiv.scrollHeight  // 元素内容高度
oDiv.scrollWidth   // 元素内容宽度
  oDiv.addEventListener('scroll', function () {
      //获取的数值其实就是滚动条滚动的距离
      console.log(oDiv.scrollTop);
  })
  window.pageYOffset //页面被卷去的高度 

window.scrollTo(X,Y) // 可把内容滚动到指定的坐标。
```



## 元素偏移量offset 

**offset系列常用属性**  

| offset系列属性       | 作用                                                         |
| -------------------- | ------------------------------------------------------------ |
| element.offsetParent | 返回作为该元素带有定位的父级元素，如果父级元素都没有定位则返回body |
| element.offsetTop    | 返回元素相对带有定位父元素上方的偏移，如果没有父级或者父级没有定位就以body的距离为准 |
| element.offsetLeft   | 返回元素相对带有定位父元素左边的偏移，如果没有父级或者父级没有定位就以body的距离为准 |
| element.offsetWidth  | 返回自身包括padding、border、内容区的宽度，返回数值不带单位  |
| element.offsetHeight | 返回自身包括padding、border、内容区的高度，返回数值不带单位； |

## offset和style的区别

| offset                                        | style                                       |
| --------------------------------------------- | ------------------------------------------- |
| 可以得到任意样式表中的样式值                  | 只能得到行内样式表中的样式值                |
| 获得的数值没有单位                            | 获得的是带有单位的字符串                    |
| offsetWidth 包含padding+border+width          | style.width获得不包含padding和border的值    |
| offsetWidth等属性是只读属性，只能获取不能赋值 | style.width是可读写属性，可以获得也可以赋值 |
| 我们想要获取元素大小和位置，用offset          | 我们只想给元素更改值，用style               |

## 元素可视区client系列

client客户端，可以用相关属性来获取元素的可视区的相关信息，通过client的相关属性可以动态得到该元素的边框大小。元素大小；

| client属性           | 作用                                                         |
| -------------------- | ------------------------------------------------------------ |
| element.clientTop    | 返回元素上边框的大小                                         |
| element.clientLeft   | 返回元素左边边框的大小                                       |
| element.clientWidth  | 返回自身包括padding。内容的宽度，不含边框，返回数值不带单位  |
| element.clientHeight | 返回自身包括padding、内容区的高度，不含边框，返回数值不带单位 |

注意：获取边框我们不常用；

## 元素scroll系列属性

scroll滚动，使用相关属性可以动态获取元素的大小和滚动距离等；

| scroll属性           | 作用                                               |
| -------------------- | -------------------------------------------------- |
| element.scrollTop    | 返回被卷上去的上侧距离，返回数值不带单位           |
| element.scrollLeft   | 返回被卷去的左侧距离，返回数值不带单位             |
| element.scrollWidth  | 返回自身实际的宽度，不含边框，返回的是数值不带单位 |
| element.scrollHeight | 返回自身实际的高度，不含边框，返回数值不带单位     |

```html
<script>
    var oDiv = document.querySelector('.box');
    console.log(oDiv.scrollHeight);
    console.log(oDiv.scrollWidth);
    oDiv.addEventListener('scroll', function () {
        //获取的数值其实就是滚动条滚动的距离
        console.log(oDiv.scrollTop);
    })
</script>
```





## 移动端touch触摸事件

**常见事件：**

| 触屏touch事件 | 说明                          |
| ------------- | ----------------------------- |
| touchstart    | 手指触摸到一个DOM元素时触发   |
| touchmove     | 手指在一个DOM元素上滑动是触发 |
| touchend      | 手指从一个DOM元素上移开时触发 |

## 触摸事件对象（TouchEvent）

TouchEvent是一类描述手指在触摸平面（触摸屏、触摸板）的状态变化事件。这类事件用于描述一个或多个触点。使用开发者可以检测触点的移动，触电的增加和减少；touchstart、touchmove/touchend三个事件都会各自有事件对象；

触摸事件对象重点我们看三个常见对象列表：

| 触摸列表         | 说明                                             |
| ---------------- | ------------------------------------------------ |
| e.touches        | 正在触摸屏幕的所有手指的一个列表                 |
| e.targetTouches⭐ | 正在触摸当前DOM元素上的手指的一个列表            |
| e.changedTouches | 手指状态发生了改变的列表，从无到有，从有到无变化 |

```
实际开发中：

01、touches谷歌模拟器只能只有一个触摸点，但是在真机中有几个手指头触摸了列表中就会有几个；

02、targetTouches 是触摸DOM元素；

03、如果touches和targetTouches 侦听的是一个DOM元素，他们两显示的结果是一样的；

04、当手指离开屏幕的时候，就没有了 touches 和 targetTouches,但是会有changedTouches;

**常用的触摸事件对象**（重点）

实际开发中我们最常用的触摸事件对象是targetTouches,因为我们一般都是触摸元素。

我们可以通过targetTouches的索引值获取在触摸DOM元素的时候，对应的手指相关信息；

比如：targetTouches[0] 表示得到正在触摸DOM元素的第一个手指的相关信息，比如手指的坐标等等；
```





## 窗口加载事件

### 传统的load方法

```js
window.onload =function(){} 
window.addEventListener('load',function(){})

window.onload是窗口（页面）加载事件，当文档内容完全加载完成会触发该事件（包括图像、脚本文件、css文件等），再去调用的处理函数；

#注意：

01、有了window.onload我么就可以把js代码写到页面元素的上方，因为onload是等页面内容全部加载完毕再去执行处理函数的；

02、window.onload是传统的注册事件方式只能写一次，如果有多个，只有最好一个生效；

03、如果使用addEventListener则没有限制可以加多个；
```

### 高版本浏览器兼容窗口加载事件⭐

```javascript
document.addEventListener('DOMContentLoaded',function(){})

DOMContentLoaded事件触发时，仅当DOM加载完成，不包括样式表、图片、flash等等；

IE9以上的浏览器才支持

如果页面的图片很多的话，从用户访问到onload触发可能需要较长的时间，交互效果就不能实现，必然会影响用户体验，此时用DOMcontentLoaded事件比较合适；（加载完一个文档就去执行不需要等全部加载完）
```

## 调整窗口大小事件

```js
window.onresize = function(){}

window.addEventListener('resize',function(){});

window.onresize是调整窗口大小加载事件，当触发时就调用的处理函数；

注意：

01、只要窗口大小发生像素变化，就会触发这个事件；

02、我们经常利用这个事件完成响应式布局，window.innerWidth可以获取当前屏幕的宽度；
```



## 获取页面滚动距离

window.pageYOffset 可以获得页面document被卷去的头部；

window.pageXOffset  可以获取页面document被卷去的左侧；

```js
<script>
    document.addEventListener('scroll', function () {
        console.log(window.pageYOffset);
    })
</script>
```



## IntersectionObserver

```react
  useEffect(() => {
    // 监听图片
    const current = imgRef.current!

    const observer = new IntersectionObserver(([{ isIntersecting }]) => {
      if (isIntersecting) {
        // 图片在可视区
        current.src = current.dataset.src!
        // 取消监听
        observer.unobserve(current)
      }
    })
    observer.observe(current)
  }, [])
```

