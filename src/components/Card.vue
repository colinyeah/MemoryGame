<style scoped>
.card{
    width: 150px;
    height: 150px;
    position: relative;
    cursor: pointer;
    transition: .40s;
    -webkit-transform-style: preserve-3d;
}
.flip .card-back{
    z-index: 1;
}
.flip .card-front{
    z-index: 2
}
.flip{
    transform: rotateY(180deg);
    transition: 0.4s;
}
.card-back{
    width: 100%;
    height: 100%;
    background: url(../../static/img/back.jpg) no-repeat;
    position: absolute;
    top: 0;
    left: 0;
    background-size: 100%;
    z-index: 2
}
.card-front{
    transform: rotateY(-180deg);
    width: 100%;
    height: 100%;
    background-repeat: no-repeat;
    position: absolute;
    top: 0;
    left: 0;

    background-color: white;
    background-size: 100%;
}
</style>
<template>
    <div>
        <div class="card" @click="clickCard" v-bind:class="{ flip: isFlip }">
            <div class="card-back"></div>
            <div class="card-front" ref="cardFace"></div>
        </div>
    </div>
</template>

<script>
// import pic from '../static/img/dodgetocat_v2.png'
export default {
  name: 'card',
  props: {
    onClickCard:{
        type:Function,
        default: () => {}
    },
    dataId:{
        default:0
    },
    isStart:{
      type:Boolean,
      default:false
    }
  },
  data () {
    return {
      msg:'',
      isFlip:false,
      pic:['../../static/img/dodgetocat_v2.png',
           '../../static/img/megacat-2.png',
           '../../static/img/steroidtocat.png',
           '../../static/img/gracehoppertocat.jpg',
           '../../static/img/ironcat.jpg',
           '../../static/img/kimonotocat.png',
           '../../static/img/linktocat.jpg',
           '../../static/img/mcefeeline.jpg',
           '../../static/img/octocat-de-los-muertos.jpg',
           '../../static/img/plumber.jpg',
           '../../static/img/saritocat.png',
           '../../static/img/spidertocat.png'
      ]
    }
  },
  methods:{
    clickCard(){
        //点击牌操作
        if(!this.isStart){
          //未开始不能点击
          return;
        }
        if(!this.isFlip){
            this.onClickCard(this);
            this.flip();
        }
    },
    flip(){
        //翻牌操作
        this.isFlip = !this.isFlip;
    }

  },
  watch: {
    dataId(newVal,oldVal){
        //在dataId改变的时候改变background的图片
        console.log("1212");
        let index = this.dataId;
        let imgUrl = this.pic[index-1];
        this.$refs.cardFace.style.backgroundImage = 'url('+imgUrl+')';
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

