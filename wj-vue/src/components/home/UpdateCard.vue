<template>
  <el-card class="box-card">
    <div slot="header" class="clearfix">
      <span style="font-weight: bold;font-size: 20px">最近更新</span>
      <!--<el-button style="float: right; padding: 3px 0" type="text">修改</el-button>-->
    </div>
    <div class="block">
<!--      在这里遍历时，因为我们想要展示的结果是新增加的文字显示在最上面因此需要采用reverse逆序遍历-->
      <el-timeline v-for="item in items.reverse()" key="item.id">
        <el-timeline-item :timestamp="item.time" placement="top">
          <el-card>
            <h4>{{item.title}}</h4>
            <p>kdmvp 提交于 {{item.time}}</p>
          </el-card>
        </el-timeline-item>

      </el-timeline>
    </div>
  </el-card>
</template>

<script>
  export default {
    name: 'UpdateCard',
    data:function (){
      return{
        items:[
          {
            id:1,
            title:"网站首页建设",
            time:"2022/8/17"
          },
          {
            id:2,
            title:"图书页面的初步完善",
            time:"2022/8/18"
          }
        ]
      }
    }
  }
</script>

<style scoped>
  .box-card {
    margin-top: 5px;
    width: 988px;
    height: 1050px;
    text-align: left;
  }
</style>
