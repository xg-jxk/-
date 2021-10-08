# cropperjs

## 头条案例

```vue
 <!-- cell   点击单元格  触发@change="onInputChange"   -->
<input ref="inp" type="file" hidden @change="onInputChange" />
<van-cell title="头像" is-link @click="$refs.inp.click()">
	<van-image class="avatar" fit="cover" round :src="userInfo.photo" />
</van-cell>
 <!--  弹出层   -->
<van-popup
    v-model="isEditAvatar"
    position="bottom"
    :style="{ height: '100%' }"
>
	<UpdateAvatar
		:img="imgUrl"
		@close="isEditAvatar = false"
		v-model="userInfo.photo"
		v-if="isEditAvatar"
	/>
</van-popup>


<script>
      // 触发file input
    onInputChange (e) {
      const file = e.target.files[0]
      console.log(file)
      this.imgUrl = URL.createObjectURL(file)
      this.isEditAvatar = true  // 弹出层
      e.target.value = ''
    }
</script>



------------------- 子组件
<template>
  <div class="update-avatar">
    <img :src="img" id="image" />
    <div class="toolbar">
      <span @click="$emit('close')">取消</span>
      <span @click="onConfirm">完成</span>
    </div>
  </div>
</template>

<script>
import { editUserAvatar } from '@/api/user.js'
import 'cropperjs/dist/cropper.css'
import Cropper from 'cropperjs'
export default {
  props: {
    // 用户选择的img
    img: {
      type: String,
      required: true
    },
    // 后台的img头像
    value: {
      type: String,
      required: true
    }
  },
  mounted () {
    const image = document.getElementById('image')
    this.cropper = new Cropper(image, {
      viewMode: 1, // 只能在裁剪的图片范围内移动
      dragMode: 'move', // 画布和图片都可以移动
      aspectRatio: 1, // 裁剪区默认正方形
      autoCropArea: 1, // 自动调整裁剪图片
      cropBoxMovable: false, // 禁止裁剪区移动
      cropBoxResizable: false, // 禁止裁剪区缩放
      background: false // 关闭默认背景
    })
    console.log(this)
  },
  methods: {
    onConfirm () {
      try {
        this.cropper.getCroppedCanvas().toBlob(async blob => {
          const formData = new FormData()
          formData.append('photo', blob)
          const { data } = await editUserAvatar(formData)
          console.log(data)
          this.$toast.success('更新头像成功')
          this.$emit('close')
          this.$emit('input', data.data.photo)
        })
      } catch (err) {
        this.$toast.success('更新失败,请稍后再试')
      }
    },
    save () {
      // 数据交给后台,让服务器裁剪
      // console.log(this.cropper.getData())
      // 客服端裁剪
      // 通过getCroppedCanvas()方法得到一个canvas元素,再通过canvas的toBlob方法转化为二进制对象
      this.cropper.getCroppedCanvas().toBlob(blob => {
        console.log(blob)
      })
    }
  }
}
</script>
```

## 大事件后台案例

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Document</title>
        <link rel="stylesheet" href="/assets/lib/layui/css/layui.css" />
        <link rel="stylesheet" href="/assets/lib/cropper/cropper.css" />
        <link rel="stylesheet" href="/assets/css/user/user_avatar.css" />
    </head>
    <body>
        <!-- 卡片区域 -->
        <div class="layui-card">
            <div class="layui-card-header">更换头像</div>
            <div class="layui-card-body">
                <!-- 第一行的图片裁剪和预览区域 -->
                <div class="row1">
                    <!-- 图片裁剪区域 -->
                    <div class="cropper-box">
                        <!-- 这个 img 标签很重要，将来会把它初始化为裁剪区域 -->
                        <img id="image" src="/assets/images/sample.jpg" />
                    </div>
                    <!-- 图片的预览区域 -->
                    <div class="preview-box">
                        <div>
                            <!-- 宽高为 100px 的预览区域 -->
                            <div class="img-preview w100"></div>
                            <p class="size">100 x 100</p>
                        </div>
                        <div>
                            <!-- 宽高为 50px 的预览区域 -->
                            <div class="img-preview w50"></div>
                            <p class="size">50 x 50</p>
                        </div>
                    </div>
                </div>
                <!-- 第二行的按钮区域 -->
                <div class="row2">
                    <!-- accept属性可以指定允许用户选择什么类型的文件 -->
                    <input type="file" id="file" accept="image/png,image/jpeg" />
                    <button type="button" class="layui-btn" id="btnsc">上传</button>
                    <button type="button" class="layui-btn layui-btn-danger" id="btnok">
                        确定
                    </button>
                </div>
            </div>
        </div>
        <script src="/assets/lib/layui/layui.all.js"></script>
        <script src="/assets/lib/jquery.js"></script>
        <script src="/assets/js/baseAPI.js"></script>
        <script src="/assets/lib/cropper/Cropper.js"></script>
        <script src="/assets/lib/cropper/jquery-cropper.js"></script>
        <script src="/assets/js/user/user_avatar.js"></script>
    </body>
</html>

```

**user_avatar.js**

```js
$(function () {
    const { form, layer } = layui;
    // 1.1 获取裁剪区域的 DOM 元素
    var $image = $("#image");
    // 1.2 配置选项
    const options = {
        // 纵横比
        aspectRatio: 1,
        // 指定预览区域
        preview: ".img-preview",
    };

    // 1.3 创建裁剪区域
    $image.cropper(options);

    $("#btnsc").on("click", function () {
        $("#file").click();
        // 为文件选择框绑定 change 事件
    });
    $("#file").on("change", function (e) {
        // 获取用户选择的文件
        var filelist = e.target.files;
        if (filelist.length === 0) {
            return layer.msg("请选择照片！");
        }

        // 1. 拿到用户选择的文件
        var file = e.target.files[0];
        // 2. 将文件，转化为路径
        var imgURL = URL.createObjectURL(file);
        // 3. 重新初始化裁剪区域
        $image
            .cropper("destroy") // 销毁旧的裁剪区域
            .attr("src", imgURL) // 重新设置图片路径
            .cropper(options); // 重新初始化裁剪区域
    });
    $("#btnok").on("click", function () {
        var dataURL = $image
            .cropper("getCroppedCanvas", {
                // 创建一个 Canvas 画布
                width: 100,
                height: 100,
            })
            .toDataURL("image/png");
        $.ajax({
            method: "POST",
            url: "/my/update/avatar",
            data: {
                avatar: dataURL,
            },
            success: function (res) {
                if (res.status !== 0) {
                    return layer.msg("更换头像失败");
                }
                layer.msg("更换头像成功");
                window.parent.getUserInfo();
            },
        });
    });
});
```

