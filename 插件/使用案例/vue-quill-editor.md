# vue-quill-editor

## vue-shop案例

```js
// 入口文件引入
import VueQuillEditor from 'vue-quill-editor'

// require styles
import 'quill/dist/quill.core.css'
import 'quill/dist/quill.snow.css'
import 'quill/dist/quill.bubble.css'

Vue.use(VueQuillEditor /* { default global options } */)

--------------
// 组件中使用

<quill-editor v-model="addForm.goods_introduce"></quill-editor>
```



## 面面案例

```js
// 入口文件引入
import VueQuillEditor from 'vue-quill-editor'

// require styles
import 'quill/dist/quill.core.css'
import 'quill/dist/quill.snow.css'
import 'quill/dist/quill.bubble.css'

Vue.use(VueQuillEditor /* { default global options } */)


// 富文本配置
editorOption: {
  placeholder: '',
  modules: {
    toolbar: [
      ['bold', 'italic', 'underline', 'strike'], // 加粗，斜体，下划线，删除线
      // [{ header: 1 }, { header: 2 }], // 标题，键值对的形式；1、2表示字体大小
      [{ list: 'ordered' }, { list: 'bullet' }], // 列表
      ['blockquote', 'code-block'], // 引用，代码块
      // [{ script: 'sub' }, { script: 'super' }], // 上下标
      // [{ indent: '-1' }, { indent: '+1' }], // 缩进
      // [{ direction: 'rtl' }], // 文本方向
      // [{ size: ['small', false, 'large', 'huge'] }], // 字体大小
      // [{ header: [1, 2, 3, 4, 5, 6, false] }], // 几级标题
      // [{ color: [] }, { background: [] }], // 字体颜色，字体背景颜色
      // [{ font: [] }], // 字体
      // [{ align: [] }], // 对齐方式
      // ['clean'], // 清除字体样式
      ['image', 'link'] // 上传图片、上传视频
    ]
  }
},

------- 

<!-- 富文本编辑器 -->
<quill-editor v-model="editForm.articleBody" :options="editorOption" ref="myTextEditor">
</quill-editor>
```

