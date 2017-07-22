<style scoped>
.time-count{
  display: inline-block;
}
</style>
<template>
    <div class="time-count">
        {{label}}:{{timeStr}}
    </div>
</template>

<script>
export default {
  name: 'Timecount',
  props: {
    label:{
      default:'time'
    }
  },
  data () {
    return {
      timeStr:'00:00',
      startTime:0,
      timer:null,
    }
  },
  methods:{
    startCount (){
      //开始计时
      this.startTime = (new Date()).getTime();
      this.timer = setInterval(()=>{
        this.computeTime();
      },1000);
    },
    computeTime (){
      //计算时间
      let nowTime = (new Date()).getTime();
      let distance = nowTime-this.startTime;
      let second = parseInt((distance/1000)%60);
      let minute = parseInt((distance/1000)/60);
      this.setTimeStr(second,minute);
    },
    setTimeStr (second,minute){
      //格式化时间表现形式
      let secondStr = second<10 ? '0'+second : second;
      let minuteStr = minute<10 ? '0'+minute : minute;
      this.timeStr = minuteStr+':'+secondStr;
    },
    stopCount (){
      //停止计时
      clearInterval(this.timer);
    }

  },
  mounted (){
    
  },
  destroyed(){
    //记得清楚定时器
    if(this.timer){
        clearInterval(this.timer);
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

