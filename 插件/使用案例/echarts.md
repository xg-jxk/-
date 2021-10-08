# echarts

**柱状图**

```vue
<template>
  <div>
    <!-- 面包屑导航 -->
    <el-breadcrumb separator="/">
      <el-breadcrumb-item :to="{ path: '/welcome' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>数据统计</el-breadcrumb-item>
      <el-breadcrumb-item>数据报表</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 卡片 -->
    <el-card>
      <div id="main" style="width: 600px;height:400px;"></div>
    </el-card>
  </div>
</template>

<script>
import _ from 'lodash'
import * as echarts from 'echarts'
export default {
  data () {
    return {}
  },
  methods: {},
  computed: {},
  async mounted () {
    // 基于准备好的dom，初始化echarts实例
    var myChart = echarts.init(document.getElementById('main'))
    const options = {
      title: {
        text: '用户来源'
      },
      tooltip: {
        trigger: 'axis',
        axisPointer: {
          type: 'cross',
          label: {
            backgroundColor: '#E9EEF3'
          }
        }
      },
      grid: {
        left: '3%',
        right: '4%',
        bottom: '3%',
        containLabel: true
      },
      xAxis: [
        {
          boundaryGap: false
        }
      ],
      yAxis: [
        {
          type: 'value'
        }
      ]
    }
    const { data: res } = await this.$http.get('reports/type/1')
    if (res.meta.status !== 200) return this.$message.error('获取报表数据失败')
	  // merge 合并对象
    const option = _.merge(options, res.data)
    // 绘制图表
    myChart.setOption(option)
  }
}
</script>

<style scoped lang='less'>
</style>

```

