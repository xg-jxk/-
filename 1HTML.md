# HTML



## grid布局

```react
display:grid

grid-template-rows:100px 100px;  //高各100px的两个盒子

grid-template-columns:100px 100px; // 宽各100px 两个盒子

grid-template-columns: repeat(3,100px);  //宽各100px 三个盒子  repeat平分

grid-template-columns: repeat(auto-fill,100px); //平分若干个100px的盒子

grid-template-columns: 1fr 1fr;   //平分两份

grid-template-areas:
      'a b c'
      'd e f'
      'h i j' ;

 
<div class="parent">
 	<div class="item" style="grid-area: a;background-color: blue;">1</div>
 	<div class="item" style="grid-area: e;background-color: green;">2</div>
	<div class="item" style="grid-area: j;background-color: red;">3</div>
</div>
```





## 新增标签

```html
<!-- 语义化标准主要是针对搜索引擎的,标签在页面中可以使用多次,移动端常用,IE9需要转换为块级元素 -->
		<!-- 头部标签 -->
    <header></header>
    <!-- 导航标签 -->
    <nav></nav>
    <!-- 内容标签 -->
    <article></article>
    <!-- 定义文档某个区域 -->
    <section></section>
    <!-- 侧边栏 -->
    <aside></aside>
    <!-- 尾部 -->
    <footer></footer>
```

## video/audio

```html
  <!-- autoplay自动播放,muted静音播放,loop循环播放,controls显示默认播放控件-->
    <video
    src="./media/mi.mp4"
    autoplay="autoplay"
    muted="muted"
    loop="loop"
    controls="controls"
    poster="./img/jyt.jpg"
  >
    您当前的浏览器不支持此H5的视频标签，请到<a href="#">“视频”</a>观看！或者更新浏览器的版本
  </video>
  <!-- 不同浏览器支持不同的音频格式，我们可以通过audio嵌套source标签实现 -->
  <audio>
    <source src="" />
  </audio>
```

```css
// 隐藏video 音量按钮
video::-webkit-media-controls-mute-button {
  display: none !important;
}

// 隐藏video 当前按钮
video::-webkit-media-controls-current-time-display {
  display: none !important;
}

// 隐藏video 总时间
video::-webkit-media-controls-time-remaining-display {
  display: none !important;
}

// 隐藏video 全屏按钮
video::-webkit-media-controls-fullscreen-button {
  display: none !important;
}

/全屏按钮
video::-webkit-media-controls-fullscreen-button {
    display: none;
}
//播放按钮
video::-webkit-media-controls-play-button {
    display: none;
}
//进度条
video::-webkit-media-controls-timeline {
    display: none;
}
//观看的当前时间
video::-webkit-media-controls-current-time-display{
    display: none;           
}
//剩余时间
video::-webkit-media-controls-time-remaining-display {
    display: none;           
}
//音量按钮
video::-webkit-media-controls-mute-button {
    display: none;           
}
video::-webkit-media-controls-toggle-closed-captions-button {
    display: none;           
}
//音量的控制条
video::-webkit-media-controls-volume-slider {
    display: none;           
}
//所有控件
video::-webkit-media-controls-enclosure{
    display: none;
}
```

## input表单

```html
    <!-- type=“email” 限制用户输入的必须是Email邮箱 -->
    邮箱：<input type="email">
    <!-- type=“url” 限制用户输入的必须是URL类型 -->
    网址：<input type="url">
    <!-- type=“date” 用户点击弹出对应的日期设置控件 -->
    日期：<input type="date">
    <!-- type=“time” 用户可以设置对应的时间 -->
    时间：<input type="time">
    <!-- type=“month” 用户点击弹出对应的月份设置控件 -->
    月份：<input type="month">
    <!-- type=“week” 用户点击弹出对应的周设置控件 -->
    周数：<input type="week">
    <!-- type=“number” 限制用户输入的必须是数字l类型，字母e除外 -->
    数字：<input type="number">
    <!-- type=“tel” 限制用户输入的必须是电话号码 -->
    电话：<input type="tel">
    <!-- type=“search” 搜索框功能 -->
    搜索框：<input type="search">
    <!-- type=“color” 生成一个颜色生成表单 -->
    颜色：<input type="color">
  <!-- 常用表单属性 
  1.required 取值 required 强制用户该表单内容不能为空，必填 
  2.autofocus 取值 autofocus自动获取表单光标焦点，页面加载完指定到表单 
  3.multiple 取值 multiple 实现同时提交多个文件
  4.autocomplete 取值 on或者off   off关闭表单历史提示记录
  将之前输入过的内容进行提示显示，默认打开，off关闭,该属性必须配合name属性才能生效提交 
	5.accept="image/png,image/jpeg" accept属性可以指定允许用户选择什么类型的文件-->
```





## TDK和视口标签

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
      
    <!-- width属性，用来设置视口的宽度，一般设置为device-width设备自己的宽度；
    initial-scale 初始缩放比，默认是1.0；
    maximum-scale 最大缩放比，一般设置最大允许缩放比例为1.0；
    minimum-scale  最小缩放比，一般设置最小允许缩放比例为1.0；
    user-scalable  用户是否可以进行缩放，一般禁止用户进行缩放取值为0或者no，如果允许用户缩放设置yes或     者1；-->
    <meta name="viewport" content="width=device-width, initial-scale=1.0,maximumscale=1.0,minimum-scale=1.0,user-scalable=0">
      
    <!-- 以最高级别的可用模式显示内容 -->
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
      
    <!-- 网站的标题-title -->
    <title>index首页</title>
      
    <!-- 网站的关键描述介绍-description -->
    <meta content="传智教育（“传智播客”全新升级为“传智教育”）专注IT培训,提供多种IT培训服务，如：java培训、前端开发培训、大数据培训、人工智能培训、python培训、web前端培训、软件测试培训、ui设计培训、移动开发培训、新媒体运营培训、产品经理培训IT培训服务等，是好口碑的IT培训机构。" name="description" />  
      
      <!-- 网站的主要关键词keywords -->
    <meta content="IT培训,IT培训机构,Java培训,人工智能培训,Python培训,IT培训学校,大数据培训,UI设计培训,前端开发培训,web前端培训,软件测试培训,产品经理培训" name="keywords"   />
```

