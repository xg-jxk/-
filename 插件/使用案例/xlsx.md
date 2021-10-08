# xlsx

## 人资案例

```js
// 全局注册  将vue-element-admin提供的导入功能新建一个组件，位置： `src/components/index.js`
import UploadExcel from './UploadExcel'
export default {
  install(Vue) {
    Vue.component('UploadExcel', UploadExcel) // 注册导入excel组件
  }
}
```

**1.准备组件结构样式**

将vue-element-admin提供的导入功能新建一个组件，位置： `src/components/UploadExcel/index.js`

```vue
<template>
  <div class="upload-excel">
    <div class="btn-upload">
      <el-button :loading="loading" size="mini" type="primary" @click="handleUpload">
        点击上传
      </el-button>
    </div>

    <input ref="excel-upload-input" class="excel-upload-input" type="file" accept=".xlsx, .xls" @change="handleClick">
    <div class="drop" @drop="handleDrop" @dragover="handleDragover" @dragenter="handleDragover">
      <i class="el-icon-upload" />
      <span>将文件拖到此处</span>
    </div>
  </div>
</template>

<script>
import XLSX from 'xlsx'
export default {
  props: {
    beforeUpload: Function, // eslint-disable-line
    onSuccess: Function// eslint-disable-line
  },
  data() {
    return {
      loading: false,
      excelData: {
        header: null,
        results: null
      }
    }
  },
  methods: {
    // 上传成功 返回excelData
    generateData({ header, results }) {
      this.excelData.header = header
      this.excelData.results = results
      this.onSuccess && this.onSuccess(this.excelData)
    },
    handleDrop(e) {
      e.stopPropagation()
      e.preventDefault()
      if (this.loading) return
      const files = e.dataTransfer.files
      if (files.length !== 1) {
        this.$message.error('Only support uploading one file!')
        return
      }
      const rawFile = files[0] // only use files[0]
      if (!this.isExcel(rawFile)) {
        this.$message.error('Only supports upload .xlsx, .xls, .csv suffix files')
        return false
      }
      this.upload(rawFile)
      e.stopPropagation()
      e.preventDefault()
    },
    handleDragover(e) {
      e.stopPropagation()
      e.preventDefault()
      e.dataTransfer.dropEffect = 'copy'
    },
    // 点击按钮弹出input file
    handleUpload() {
      this.$refs['excel-upload-input'].click()
    },
    // 选择文件事件
    handleClick(e) {
      const files = e.target.files
      const rawFile = files[0] // only use files[0]
      if (!rawFile) return // 没有选择直接返回
      this.upload(rawFile)
    },
    // 上传文件
    upload(rawFile) {
      this.$refs['excel-upload-input'].value = null // fix can't select the same excel
      if (!this.beforeUpload) {
        this.readerData(rawFile)
        return
      }
      const before = this.beforeUpload(rawFile)
      if (before) {
        this.readerData(rawFile)
      }
    },
    readerData(rawFile) {
      this.loading = true
      return new Promise((resolve, reject) => {
        const reader = new FileReader()
        reader.onload = e => {
          const data = e.target.result
          const workbook = XLSX.read(data, { type: 'array' }) // XLSX 第三方包
          const firstSheetName = workbook.SheetNames[0]
          const worksheet = workbook.Sheets[firstSheetName]
          const header = this.getHeaderRow(worksheet)
          const results = XLSX.utils.sheet_to_json(worksheet)
          this.generateData({ header, results })
          this.loading = false
          resolve()
        }
        reader.readAsArrayBuffer(rawFile)
      })
    },
    getHeaderRow(sheet) {
      const headers = []
      const range = XLSX.utils.decode_range(sheet['!ref'])
      let C
      const R = range.s.r
      /* start in the first row */
      for (C = range.s.c; C <= range.e.c; ++C) { /* walk every column in the range */
        const cell = sheet[XLSX.utils.encode_cell({ c: C, r: R })]
        /* find the cell in the first row */
        let hdr = 'UNKNOWN ' + C // <-- replace with your desired default
        if (cell && cell.t) hdr = XLSX.utils.format_cell(cell)
        headers.push(hdr)
      }
      return headers
    },
    isExcel(file) {
      return /\.(xlsx|xls|csv)$/.test(file.name)
    }
  }
}
</script>

<style scoped lang="scss">
.upload-excel {
  display: flex;
  justify-content: center;
   margin-top: 100px;
   .excel-upload-input{
       display: none;
        z-index: -9999;
     }
   .btn-upload , .drop{
      border: 1px dashed #bbb;
      width: 350px;
      height: 160px;
      text-align: center;
      line-height: 160px;
   }
   .drop{
       line-height: 80px;
       color: #bbb;
      i {
        font-size: 60px;
        display: block;
      }
   }
}
</style>
```

**创建import路由组件** **`src/views/import/index.vue`**

```vue
<template>
  <!-- 公共导入组件 -->
  <upload-excel :on-success="success" />
</template>
```

**新建一个公共的导入页面，挂载路由** **`src/router/index.js`**

```js
{
    path: '/import',
    component: Layout,
    hidden: true, // 隐藏在左侧菜单中
    children: [{
      path: '', // 二级路由path什么都不写 表示二级默认路由
      component: () => import('@/views/import')
    }]
  },
```

**分析excel导入代码，封装接口**

> 封装导入员工的api接口  

```js
//  封装一个导入员工的接口
export function importEmployee(data) {
  return request({
    url: '/sys/user/batch',
    method: 'post',
    data
  })
}
```

**实现excel导入**

**获取导入的excel数据, 导入excel接口  **

 **`src/views/import/index.vue`定义success方法**

```js
    // { header, results }   
    async success({ header, results }) {
        const userRelations = {
          '入职日期': 'timeOfEntry',
          '手机号': 'mobile',
          '姓名': 'username',
          '转正日期': 'correctionTime',
          '工号': 'workNumber'
        }
        const arr = []
        // 遍历所有的数组
        results.forEach(item => {
          // 需要将每一个条数据里面的中文都换成英文
          const userInfo = {}
          Object.keys(item).forEach(key => {
            if (userRelations[key] === 'timeOfEntry' || userRelations[key] === 'correctionTime') {
              // 处理时间数据 new Date 第二个参数加 /
              userInfo[userRelations[key]] = new Date(this.formatDate(item[key], '/'))
              return
            }
            // key是当前的中文名 找到对应的英文名
            userInfo[userRelations[key]] = item[key]
          })
          // 最终userInfo变成了全是英文
          arr.push(userInfo)
        })
        await importEmployee(arr) // 调用导入接口
        this.$message.success('导入成功')
      
      this.$router.back() // 回到上一页
    },
    // 转换为excel需要的日期格式
    formatDate(numb, format) {
      const time = new Date((numb - 1) * 24 * 3600000 + 1)
      time.setYear(time.getFullYear() - 70)
      const year = time.getFullYear() + ''
      const month = time.getMonth() + 1 + ''
      const date = time.getDate() - 1 + ''
      if (format && format.length === 1) {
        return year + format + month + format + date
      }
      return year + (month < 10 ? '0' + month : month) + (date < 10 ? '0' + date : date)
    }
```

