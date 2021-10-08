# sdk

**封装组件**

`src/components/ImageUpload/index.js`

```vue
<template>
  <div>
    <!-- action 必填项-->
    <el-upload
      :on-preview="preview"
      :on-remove="handleRemove"
      :on-change="changeFile"
      :before-upload="beforeUpload"
      :file-list="fileList"
      list-type="picture-card"
      action="#"
      :limit="1"
      :class="{ disabled: fileComputed }"
      :http-request="upload"
    >
      <i class="el-icon-plus" />
    </el-upload>
     <!-- 进度条 -->
    <el-progress v-if="showPercent" style="width: 180px" :percentage="percent" />
     <!-- 预览弹层 -->
    <el-dialog title="图片预览" :visible.sync="showDialog">
      <!-- percentage进度百分比 -->
      <img :src="imgUrl" style="width:100%" alt="">
    </el-dialog>
  </div>
</template>

<script>
import COS from 'cos-js-sdk-v5' // 引入腾讯云的包
// SecretId: AKIDHNEp9CzVkiRt50e17wUnoZ2GDtHVTAaj
// SecretKey: uXu0qPhiTyB7mfIejY6NN6YjdGelbATV
// 需要实例化
const cos = new COS({
  SecretId: 'AKIDHNEp9CzVkiRt50e17wUnoZ2GDtHVTAaj',
  SecretKey: 'uXu0qPhiTyB7mfIejY6NN6YjdGelbATV'
}) // 实例化的包 已经具有了上传的能力 可以上传到该账号里面的存储桶了
export default {
  data() {
    return {
      fileList: [], // 图片地址设置为数组 [{url:'图片地址'}]
      showDialog: false, // 控制显示弹层
      imgUrl: '',
      currentFileUid: '', // 用户选择当前上传的图片id
      percent: 0, // 上传进度
      showPercent: false // 默认不显示进度条
    }
  },
  computed: {
    // 判断用户是否选择图片 选择了一张图片就隐藏图片添加按钮
    fileComputed() {
      return this.fileList.length === 1
    }
  },
  methods: {
    // 点击预览 file.url是点击的图片地址
    preview(file) {
      this.imgUrl = file.url
      this.showDialog = true
    },
    // 点击删除图片
    // file是要删除的文件 fileList是删过之后的文件数组
    handleRemove(file, fileList) {
      //  过滤点击的图片
      this.fileList = this.fileList.filter(item => item.uid !== file.uid)
      // 第二种写法
      // this.fileList = fileList
    },
    // 修改文件时触发
    // 此钩子可能会触发多次不能用push
    changeFile(file, fileList) {
      this.fileList = fileList.map(item => item)
    },
    // 上传之前调用
    beforeUpload(file) {
      const types = ['image/jpeg', 'image/gif', 'image/bmp', 'image/png']
      // 检查文件类型
      if (!types.includes(file.type)) {
        this.$message.error('上传图片只能是 JPG、GIF、BMP、PNG 格式!')
        return false
      }
      //  检查文件大小 单位B
      const maxSize = 5 * 1024 * 1024
      if (maxSize < file.size) {
        this.$message.error('图片大小最大不能超过5M')
        return false
      }
      //  已经确定当前上传的就是当前的这个file了
      this.currentFileUid = file.uid
      this.showPercent = true
      return true
    },
    // 自定义上传动作  params.file是我们需要上传到腾讯云服务器的内容
    upload(params) {
      if (params.file) {
        //  上传文件到腾讯云
        cos.putObject({
          // 配置
          Bucket: 'xiguajxk-1306950670', // 存储桶名称
          Region: 'ap-shanghai', // 存储桶地域
          Key: params.file.name, // 文件名作为key
          StorageClass: 'STANDARD', // 此类写死
          Body: params.file, // 将本地的文件赋值给腾讯云配置
          // 进度条
          onProgress: (params) => {
            this.percent = params.percent * 100
          }
        }, (err, data) => {
          console.log(err, data)
          debugger
          // 需要判断错误与成功
          if (!err && data.statusCode === 200) {
            // 上传成功
            this.fileList = this.fileList.map(item => {
              // 将成功的地址赋值给原理的url属性
              if (item.uid === this.currentFileUid) {
                //   upload为true表示 该图片已经成功上传到服务器，地址已经是腾讯云的地址了  就不可以执行保存了
                return { url: 'http://' + data.Location, upload: true } // 将本地的地址换成腾讯云地址
              }
              return item
            })
            setTimeout(() => {
              this.showPercent = false // 隐藏进度条
              this.percent = 0 // 进度归0
            }, 2000)

            // 将腾讯云地址写入到fileList上 ，保存的时候 就可以从fileList中直接获取图片地址

            // 此时注意，我们应该记住 当前上传的是哪个图片  上传成功之后，将图片的地址赋值回去
          }
        })
      }
    }
  }
}
</script>

<style>
.disabled .el-upload--picture-card {
  display: none;
}
</style>

```

**父组件使用**

```vue
<image-upload ref="myStaffPhoto" />

<script>
export default {
  methods: { 
    // 读取用户详情信息
    async getPersonalDetail() {
      this.formData = await getPersonalDetail(this.userId) // 获取员工数据
      if (this.formData.staffPhoto) {
        this.$refs.myStaffPhoto.fileList = [{ url: this.formData.staffPhoto, upload: true }]
      }
    },
    // 保存用户详细信息
    async savePersonal() {
      const fileList = this.$refs.myStaffPhoto.fileList
      if (fileList.some(item => !item.upload)) {
        //  如果此时去找 upload为false的图片 找到了说明 有图片还没有上传完成
        this.$message.warning('您当前还有图片没有上传完成！')
        return
      }
      await updatePersonal({ ...this.formData, staffPhoto: fileList && fileList.length ? fileList[0].url : '' })
      this.$message.success('保存基础信息成功')
    },
  }
}
</script>

```

