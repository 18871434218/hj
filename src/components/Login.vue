<template>
  <div class="Login">
    <div style="text-align:center;font-size:24px; padding-top:40px"><h1>{{ msg }}</h1></div>
    <div style="text-align:center;font-size:20px"><h1>{{ msg1 }}</h1></div>
    <div>
      请输入用户名：
      <el-input
        placeholder="请输入用户名"
        v-model="input21">
      </el-input>
    </div>
    <div>
      请输入邀请码：
      <el-input
        placeholder="请输入邀请码"
        v-model="input23">
      </el-input>
    </div>

    <el-button type="primary">接入车联网</el-button>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data () {
    return {
      msg: '舜宇车联网平台',
      msg1:"车端模拟系统"
    }
  }
}
</script>

<style scoped>
</style>
