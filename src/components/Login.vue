<template>
  <div class="Login">
    <div style="text-align:center;font-size:24px; padding-top:40px"><h1>{{ msg }}</h1></div>
    <div style="text-align:center;font-size:20px"><h1>{{ msg1 }}</h1></div>
  
    <button>你是</button>
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
