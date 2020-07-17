<template>
  <div class="Home">
    <div style="text-align:center;font-size:22px; padding-top:40px"><h1>欢迎{{userName}}接入舜宇车联网</h1></div>

    <div><span style="font-size:22px; padding-top:30px; padding-left:100px">模拟接口</span></div>
  
    <el-row style="margin-top:100px">
       <el-col :lg="12"><span style="margin-left:100px;font-size:20px">设置车状态</span></el-col>
       <el-col :lg="12" style="margin-left:250px;margin-top:-20px">
         <el-switch
            v-model="CarState"
            active-text="移动"
            inactive-text="停止">
         </el-switch>
       </el-col>
    </el-row>

    <el-row style="margin-top:40px">
       <el-col :lg="12"><span style="margin-left:100px;font-size:20px;">控制器</span></el-col>
       <el-col :lg="12" style="margin-left:250px;margin-top:-20px">
         <el-switch
            v-model="Controller"
            active-text="打开"
            inactive-text="关闭">
         </el-switch>
       </el-col>
    </el-row>
       
    <el-row style="margin-top:40px">
       <el-col :lg="12"><span style="margin-left:100px;font-size:20px">激光雷达</span></el-col>
       <el-col :lg="12" style="margin-left:250px;margin-top:-20px">
         <el-switch
            v-model="Lidar"
            active-text="打开"
            inactive-text="关闭">
         </el-switch>
       </el-col>
    </el-row>

    <el-row style="margin-top:40px">
      <el-col :lg="12"><span style="margin-left:100px;font-size:20px">毫米波雷达</span></el-col>
      <el-col :lg="12" style="margin-left:250px;margin-top:-20px">
         <el-switch
            v-model="wave_radar"
            active-text="正常"
            inactive-text="故障">
         </el-switch>
      </el-col>  
    </el-row>
  </div>
</template>

<script>
import mqtt from 'mqtt'
export default {
  name: 'Home',
  inject: ["Login_username", "Login_password"],
  data () {
    return {
      userName: "",
      CarState: "停止",     //设置车状态
      Controller: "关闭",   //控制器
      Lidar: "关闭",        // 激光雷达
      wave_radar: "故障",   //毫米波雷达

      client: null,     

      message:{
        "baseState":[1.000000,40.800000,0.4],
        "vehicleInfo":[1.000000,"002",0.000000,120.199000,30.281000,40.000000,20.000000,0.500000],
        "errorInfo":[1.000000,0.000000,0.000000,0.000000,0.000000,0.000000,0.000000,0.000000]
      }
    }
  },
  
  watch:{
    CarState(val){
      /* console.log("111111111111", val);
      console.log("nisdcs",this.CarState) */
        /* console.log("1111",this.CarState) */
        if(val){
            /* console.log("65443",this.message.vehicleInfo[3]); */
             /* console.log("13213",typeof(this.message.vehicleInfo[3])); */
             this.message.vehicleInfo[3] = 120.199000 + Math.random() * 0.0005;
             this.message.vehicleInfo[4] = 30.281000 + Math.random() * 0.0005;
             this.message.vehicleInfo[5] = 40.000000 + Math.random() * 10;
            /*  console.log("1111",this.CarState) */
         };
    },

    Controller(val){
         /*  console.log(this.Controller)
          console.log("bub", val) */
         if(val){
              this.message.errorInfo[-1] = 1.000000;
             /*  console.log(this.message) */
         };
    },

    Lidar(val){
        if(val){
            this.message.errorInfo[2] = 0.000000;
        };
    },

    wave_radar(val){
       /*  console.log(val) */
        if(val){
            this.message.errorInfo[3] = 1.000000;
        }
    }
  },

 /*  methods:{
    pulish(){
        
    }
  }, */

  /* computed:{
      message(){
          if(this.CarState == "移动");{
              message.vehicleInfo[3] = Math.random(1,1000);
              message.vehicleInfo[4] = Math.random(1,1000);
              message.vehicleInfo[5] = Math.random(1,1000);
          }
          if(this.Controller == "打开"){
              message.errorInfo[-1] = 1.000000;
          }
          if(this.Lidar == "打开"){
             message.errorInfo[2] = 0.000000;
          }
          if(this.wave_radar == "正常"){
             message.errorInfo[3] = 1.000000;
          }
      }
  }, */

  mounted(){
      this.userName = this.$root.Login_username;
      
      const options = {
         connectTimeout: 40000,
         clientId: "mqtitId-_client",
         clean: true,
         keepAliveInterval: 100,
       };

      this.client = mqtt.connect("wss://www.sunnyiov.com/mqtt", options);

      this.client.on("connect", e => {
        console.log("连接成功");
        
        var topic = "testProject/clientSub/baseState";                 //发布主题
        var qtt = JSON.stringify(this.message); 


       /*  var pu = this.client.publish(topic, qtt,  { qos: 0, retain: false });
        console.log("nu", pu); */

        setInterval(this.client.publish(topic, qtt,  { qos: 0, retain: false }, function(err){
          if(err == undefined){
            console.log("Publish finished");
          }else{
            console.log("publish failed");
          }
        }), 2000);      //定时  
      }); 
      
      

      this.client.on("reconnect", error => {
        console.log("正在重连：", error);
      });

      this.client.on("error", error => {
        console.log("连接失败", error);
      });
  },
  
}
</script>

<style>
</style>
