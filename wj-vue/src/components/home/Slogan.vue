<template>
  <div>
    <el-card class="box-card">
      <div style="font-size: 20px;color: azure;white-space: nowrap">Talk is nothing, show you my code.</div>
      <a href="https://github.com/kdmvp-lin/BookStore" target="_blank">
        <img src="../../assets/img/icon/icon6.png" alt="" class="git-link">
      </a>
      <a href="https://blog.csdn.net/Neuf_Soleil" target="_blank">
        <img src="../../assets/img/icon/csdn.png" alt="" class="csdn-link">
      </a>
    </el-card>
  </div>
</template>

<script>
  export default {
    name: 'Slogan'
  }
</script>

<style scoped>
  .box-card {
    width: 100%;
    height: 100px;
    background-color: #222;
    text-align: center;
    margin-top: 5px;
  }

  .slogan {
    width: 100%;
  }

  .git-link {
    width: 53px;
    /*position: absolute;*/
  }

  .csdn-link {
    width: 40px;
    /*position: absolute;*/
    margin-bottom: 6px;
    margin-left: 20px;
  }
</style>
