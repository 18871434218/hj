<template>
  <div class="Login">
    <div style="text-align:center;font-size:24px; padding-top:40px">
      <h1>{{ msg }}</h1>
    </div>
    <div style="text-align:center;font-size:20px">
      <h1>{{ msg1 }}</h1>
    </div>
    <div style="text-align:center;font-size:20px; height:100px"></div>
    
    <div>
    <el-container>
      <el-aside style="margin-left:100px">
        <span>用户名：</span>
      </el-aside>
      <el-main style="margin-left:-250px; margin-top:-25px">
        <div style="width:158px"><el-input placeholder="请输入用户名" v-model="userName"></el-input></div>
      </el-main>
    </el-container>
    </div>

    <div style="margin-top:30px">
    <el-container>
      <el-aside style="margin-left:100px">
        <span>邀请码：</span>
      </el-aside>
      <el-main style="margin-left:-250px; margin-top:-25px">
        <div style="width:158px"><el-input placeholder="请输入邀请码" v-model="password"></el-input></div>
      </el-main>
    </el-container>
    </div>

    <el-button type="primary" @click="change" style="margin-left:170px; width:158px; margin-top:30px" >接入车联网</el-button>
  </div>
</template>

<script>
/* import mqtt, { connect } from 'mqtt' */
export default {
  inject: ["Login_username", "Login_password"],
  name: "Login",
  data() {
    return {
      msg: "舜宇车联网平台",
      msg1: "车端模拟系统",
      userName: "",
      password: "",
      client: null,

      message:{
        "baseState":[1.000000,40.800000,0.4],
        "vehicleInfo":[1.000000,"002",0.000000,120.199000,30.281000,40.000000,20.000000,0.500000],
        "errorInfo":[1.000000,0.000000,1.000000,0.000000,0.000000,0.000000,0.000000,2.000000]
        }
    };
  },
  
 /*  mounted(){
       const options = {
         connectTimeout: 40000,
         clientId: "mqtitId-_client",
         clean: true,
         keepAliveInterval: 100,
       };
       this.client = mqtt.connect("wss://www.sunnyiov.com/mqtt", options);
       
        
       this.clien.on("connect", e => {
           connect.log("连接成功：");
           
           var topic = "testProject/clientSub/baseState";         //发布主题
           this.client.publish(topic, this.message,  { qos: 0, retain: false });
       });
       
       
  }, */

  methods: {
    change() {
      if(this.userName == this.$root.Login_username && this.password == this.$root.Login_password){
        this.$router.push("/Home");
        console.log("输入信息正确");
    }else{
       alert("输入信息错误，请重新输入");
     }
    }
  }
};
</script>

<style scoped>
</style>
