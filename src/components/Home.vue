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
        "vehicleInfo":[0.000000,"002",0.000000,120.199000,30.281000,40.000000,20.000000,0.500000],
        "errorInfo":[1.000000,1.000000,1.000000,1.000000,1.000000,1.000000,1.000000,1.000000]
      },
     /*  topic: "testProject/clientSub/baseState",                 //发布主题
      qtt: null  */
    }
  },
  
  watch:{
    CarState(val){
      /* console.log("111111111111", val);
      console.log("nisdcs",this.CarState) */
        /* console.log("1111",this.CarState) */
        if(val){
             /* this.message.vehicleInfo[3] = 120.199000 + Math.random() * 0.005;
             this.message.vehicleInfo[4] = 30.281000 + Math.random() * 0.005;
             this.message.vehicleInfo[5] = 40.000000 + Math.random() * 10; */

            /*  console.log("1111",this.CarState) */
           /*  if(this.message.vehicleInfo[3] >= 128){
               this.message.vehicleInfo[3] = 120.199000;
            }
            this.message.vehicleInfo[3] = this.message.vehicleInfo[3] + Math.random() * 0.005;
            
            if(this.message.vehicleInfo[4] >= 38){
               this.message.vehicleInfo[4] = 30.281000;
            }
            this.message.vehicleInfo[4] = this.message.vehicleInfo[4] + Math.random() * 0.005;
          
            if(this.message.vehicleInfo[5] >= 30){
               this.message.vehicleInfo[5] = 40.000000;
            }
            this.message.vehicleInfo[5] = this.message.vehicleInfo[5] + Math.random() * 10;
 */
           /*  let r = 10; 

            this.message.vehicleInfo[3] = this.message.vehicleInfo[3] + Math.random() ;   
            this.message.vehicleInfo[4] = this.message.vehicleInfo[4] + Math.random(); 
            this.message.vehicleInfo[5] =   */
            var r = 10;

            var pointCount=Math.round(2*Math.PI*r);
            //循环算出每一个点的坐标 并将生成的点添加到body中（body可以是其他的容器 如div）
            for (var i=0; i<=pointCount; i++){
              var a=(i*2*Math.PI)/(pointCount);
              this.message.vehicleInfo[3]=(this.message.vehicleInfo[3]+Math.cos(a)*r);   //根据圆心 半径 和三角函数计算x
              this.message.vehicleInfo[4]=(this.message.vehicleInfo[4]+Math.sin(a)*r);   //同计算x
              
              this.message.vehicleInfo[5] =  Math.sqrt((Math.pow(Math.cos(a)*r, 2) + Math.pow(Math.sin(a)*r, 2))) / 0.002;
            };
         };
    },

    Controller(val){
         /*  console.log(this.Controller)
          console.log("bub", val) */
         if(val){
              this.message.errorInfo[-1] = 0;
             /*  console.log(this.message) */
         }else{
           this.message.errorInfo[-1] = 1;
         }
    },

    Lidar(val){
        if(val){
            this.message.errorInfo[2] = 0;
        }else{
          this.message.errorInfo[2] = 1;
        }
    },

    wave_radar(val){
       /*  console.log(val) */
        if(val){
            this.message.errorInfo[3] = 0;
        }else{
          this.message.errorInfo[3] = 1;
        }
    }
  },

 /*  methods:{
    pulish(){
        
    }
  }, */

    methods:{
        Send(){
            var topic = "testProject/clientSub/baseState";                 //发布主题
            var qtt = JSON.stringify(this.message); 

            this.client.publish(topic, qtt,  { qos: 0, retain: false }, function(err){
              if(err == undefined){
                console.log("Publish finished");
              }else{
                console.log("publish failed");
              }
            });
        }
    },

  mounted(){
      this.userName = this.$root.Login_username;
      
      const options = {
         connectTimeout: 40000,
         clientId: "mqtitId-_client",
         clean: true,
         keepAliveInterval: 5000,
       };

      this.client = mqtt.connect("wss://www.sunnyiov.com/mqtt", options);

      this.client.on("connect", e => {
        console.log("连接成功");
        
       /*  var topic = "testProject/clientSub/baseState";                 //发布主题
        var qtt = JSON.stringify(this.message);  */

       /*  var pu = this.client.publish(topic, qtt,  { qos: 0, retain: false });
        console.log("nu", pu); */

      //   setInterval(this.client.publish(topic, qtt,  { qos: 0, retain: false }, function(err){
      //     if(err == undefined){
      //       console.log("Publish finished");
      //     }else{
      //       console.log("publish failed");
      //     }
      //   }), 2000);           //定时  
      // }); 
      
      /*  setInterval(this.client.publish(topic, qtt,  { qos: 0, retain: false },
        ), 2000);           //定时  
       });  */

     /*  setInterval(this.client.publish(topic, qtt,  { qos: 0, retain: false }), 2000);           //定时  */ 

        setInterval(this.Send(), 5000);           //定时 
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
