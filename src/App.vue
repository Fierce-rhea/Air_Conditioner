<template>
  <v-app>
    <v-app-bar
      app
      dark
      height="100px"
      style="font-size: 2rem"
    >
      <v-spacer></v-spacer>
      空调
      <v-spacer></v-spacer>
    </v-app-bar>
    <v-main>
      <br>
      <div style="text-align: center;color:#999999">
        <v-icon>mdi-lightbulb</v-icon> 夏日炎炎，有我超甜！！
      </div>

      <div class="air" style="margin-top: 2rem">
        <img src="./assets/airconditioner.png" alt="x" class="picture">
        <div v-show="on">
          <p class="pattern">{{weather}}</p>
          <span class="font">{{temp}}°C</span>
        </div>
        <span class="logo">☁</span>
        <span class="dot" v-show="on"></span>
      </div>
      <div class="wind" v-show="on" transition="fade-transition">
        <span class="wind1"></span>
        <span class="wind2"></span>
        <span class="wind3"></span>
      </div>
      <v-row style="position: fixed;left:80px;top: 450px">
        <v-spacer></v-spacer>
        <v-spacer></v-spacer>
        <v-btn
                class="mx-2"
                fab
                dark
                color="#FF9800"
                large
                @click="sun"
        >
          <v-icon>mdi-weather-sunny</v-icon>
        </v-btn>
        <v-spacer></v-spacer>
        <v-btn
                class="mx-2"
                fab
                dark
                large
                color="#0091EA"
                @click="startAir"
        >
          <v-icon dark>
            mdi-power
          </v-icon>
        </v-btn>
        <v-spacer></v-spacer>
        <v-btn
                class="mx-2"
                fab
                dark
                large
                color="#0091EA"
                @click="snow"
        >
          <v-icon dark>
            mdi-snowflake
          </v-icon>
        </v-btn>
        <v-spacer></v-spacer>
        <v-spacer></v-spacer>
      </v-row>
      <v-row style="position: fixed;left:162px;top: 525px">
        <v-spacer></v-spacer>
        <v-btn
                class="mx-2"
                fab
                large
                dark
                color="indigo"
                @click="addTemp"
        >
          <v-icon dark>
            mdi-plus
          </v-icon>
        </v-btn>
        <v-spacer></v-spacer>
      </v-row>
      <br>
      <br>
      <v-row style="position: fixed;left:162px;top: 650px">
        <v-spacer></v-spacer>
        <v-btn
                class="mx-2"
                fab
                dark
                large
                color="indigo"
                @click="desTemp"
        >
          <v-icon dark>
            mdi-minus
          </v-icon>
        </v-btn>
        <v-spacer></v-spacer>
      </v-row>
      <audio ref="ding" src="https://cdn.jsdelivr.net/gh/YunYouJun/air-conditioner/public/assets/audio/di.m4a"></audio>
      <audio ref="start" @pause="onPause" src="https://cdn.jsdelivr.net/gh/YunYouJun/air-conditioner/public/assets/audio/ac-work.m4a"></audio>
      <audio ref="cont" style="transition: all 0.2s linear" src="https://cdn.jsdelivr.net/gh/YunYouJun/air-conditioner/public/assets/audio/air-extractor-fan.m4a" loop></audio>
    </v-main>
  </v-app>
</template>



<script>

export default {
  name: 'App',

  data: () => ({
      weather:"❄️",
      on:false,
      temp:26,
  }),

  methods:{
      startAir(){
        this.on=!this.on;
        this.dingPlay();
      },
      snow(){
        this.weather="❄️";
        this.$refs.ding.play();
      },
      sun(){
        this.weather="☀️";
        this.$refs.ding.play();
      },
      addTemp(){
        this.$refs.ding.play();
        this.temp++;
      },
      desTemp(){
        this.$refs.ding.play();
        this.temp--;
      },
      dingPlay(){
        this.$refs.ding.play();
        if(this.on==false)
        {
          this.$refs.start.pause();
          var v=1;
          var that=this;
          var t=setInterval(function(){
            v=v-0.1;
            if(v>=0){
              that.$refs.cont.volume = v;
            }
            else{
              clearInterval(t);
              that.$refs.cont.pause();
            }
          },200);
        }

        if(this.on==true)
        {
          this.$refs.start.currentTime=0;
          this.$refs.start.play();
        }
      },
      onPause(){
        if(this.on==true)
        {
          this.$refs.cont.volume=1;
          this.$refs.cont.play();
        }
      }
  }

};
</script>
<style>
  .air{
    position: relative;
    margin: 0 auto;
    width: 300px;
    height: 120px;
    background-color: rgb(255, 255, 255);
    border-radius: 10px 10px 0 0;
    filter: drop-shadow(0 2px 4px rgb(145, 145, 145));
  }
  .air::after{
    content: '';
    position: absolute;
    bottom: -21px;
    left: 0;
    height: 20px;
    width: 300px;
    background-color: rgb(255, 255, 255);
    border-radius: 0 0 20px 20px;
  }
  .picture{
    position: absolute;
    top: 10px;
    left: 10px;
    width: 40px;
    height: 65px;
    border-radius: 5px;
  }
  .logo{
    position: absolute;
    left: 50%;
    bottom: 3px;
    transform: translateX(-50%);
    font-size: 10px;
    color: rgb(139, 139, 139);
  }

  .font{
    position: absolute;
    right: 30px;
    top: 45px;
    width: 50px;
    height: 30px;
    line-height: 30px;
    font-size: 25px;
    color: rgb(184, 184, 184);
    transition: all 0.3s;
  }
  .dot{
    position: absolute;
    bottom: -8px;
    right: 12px;
    width: 5px;
    height: 5px;
    border-radius: 50%;
    background-color: rgb(19, 221, 53);
    z-index: 1;
    transition: all 0.3s linear;
  }
  .pattern{
    position: absolute;
    top: 25px;
    right: 35px;
    width: 20px;
    height: 20px;
    transition: all 0.3s linear;
  }
  .wind{
    position: relative;
    top: 30px;
    width: 250px;
    height: 60px;
    margin: 0 auto;
    transition: all 1.2s linear;
    animation: move 1.5s ease-in-out infinite;
  }
  .wind{
    position: relative;
    top: 30px;
    width: 250px;
    height: 60px;
    margin: 0 auto;
    transition: all 1.2s;
    animation: move 1.5s ease-in-out infinite;
  }
  @keyframes move{
    0%,100%{
      transform: translateY(0px);
    }
    50%{
      transform: translateY(5px);
    }
  }
  .wind>span{
    position: absolute;
    width: 5px;
    height: 40px;
    background-color: rgb(206, 206, 206);
  }
  .wind1{
    left: 20%;
    transform: rotate(20deg);
  }
  .wind2{
    left: 50%;
  }
  .wind3{
    left: 80%;
    transform: rotate(-20deg);
  }
</style>
