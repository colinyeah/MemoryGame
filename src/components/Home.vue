<style scoped>
.home{
    text-align: center;
}
.header{
    margin-top: 50px;
    margin-bottom: 100px;
}
.default-btn{
    border-radius: 8px;
    padding: 12px 30px;
    font-size: 24px;
    background-color: #4fc08d;
    background-image: linear-gradient(to bottom,#7df1bd,#4fc08d);
    color: #ffffff;
    display: inline-block; 
    cursor: pointer;

    -webkit-user-select:none;
    -moz-user-select:none;
    -ms-user-select:none;
    user-select:none;
}
.start-btn{
    
}
.moves-count{
    font-size: 24px;
    float: right;
    margin-top: 10px;
    margin-right: 50px;
}
.card-block{
    display: inline-block;
    margin-right: 15px;
    margin-bottom: 15px;

}
.board{
    width: 420px;
    height: 300px;
    background: #fff;
    margin: 0 auto;
    top: -500px;
    position: relative;
    border: 16px groove #7df1bd;
    border-radius: 10px;
    text-align: center;
    padding: 30px;
}
.board-title{
    font-size: 36px;
    font-style: italic;
    font-weight: 700;

}
.des{
    font-size: 28px;
    margin: 20px;
}
.score-des{
    font-size: 28px;
    margin: 20px;
    font-style: italic;
}
</style>
<template>
    <div class="home">
       <div class="header">
           <div class="start-btn default-btn" @click="replay">p l a y</div>
           <div class="moves-count">Moves:{{moves}}</div>
       </div>
       <div v-for="(it1,index1) in row">
            <div v-for="(it2,index2) in column"  class="card-block" :cardId="index1*column+index2">
                <Card :onClickCard="clickCard" :dataId="arrayList[index1*column+index2]" ref="card"></Card>
            </div>
            <br>    
       </div>
       <div class="board" v-if="showModal">
           <div class="board-title">congratulation!</div>
           <p class="des">your score:</p>
           <p class="score-des">Moves:{{moves}}</p>
           <div class="default-btn" @click="hideModal">O K</div>
       </div>
    </div>
</template>

<script>
import Card from './Card'
export default {
  components:{
     Card
  },
  name: 'home',
  data () {
    return {
      row:4,
      column:4,
      arrayList:[],

      FirstCard:{},
      SecondCard:{},
      count:0,
      moves:0,
      flipCount:0,

      timeOut:null,

      showModal:true,
    }
  },
  methods:{
    replay(){
        //重新开始玩
        this.$refs.card.map((item)=>{item.isFlip = false}); //卡片全部翻回背面
        this.arrayList = this.shuffle();  //重新洗牌
        this.FirstCard = {};
        this.SecondCard = {};
        this.count = 0;
        this.moves = 0;
        this.flipCount = 0;
    },
    clickCard(ref){
        this.moves++; //记录次数
        this.count++;
        if(this.count===1){
            this.FirstCard = ref;
        }else if(this.count === 2){
            this.SecondCard = ref;
            if(this.FirstCard.dataId !== this.SecondCard.dataId){
                //翻过来的两张牌如果不一样则翻回背面并且重置两个对象和计数器
                this.timeOut = setTimeout(()=>{
                    this.FirstCard.flip();
                    this.SecondCard.flip();
                    this.FirstCard = {};
                    this.SecondCard = {};
                    this.count = 0;
                },800);
                
            }else{
                //两张牌一样，则不用翻回背面
                this.flipCount = this.flipCount+2;
                this.FirstCard = {};
                this.SecondCard = {};
                this.count = 0;

                this.checkAllPass();
            }
            
        }else{
            //当setTimeout还未执行时再次点击卡片则直接判断和后续操作，跳过等待时间
            clearTimeout(this.timeOut);
            if(this.FirstCard.dataId !== this.SecondCard.dataId){
                this.FirstCard.flip();
                this.SecondCard.flip();
            }else{
                this.flipCount = this.flipCount+2;
                this.checkAllPass();
            }
            // console.log(">>>",this.FirstCard,this.SecondCard);
            this.FirstCard = ref;
            this.SecondCard = {};
            this.count = 1;
        }
    },
    checkAllPass(){
        //检查是否全部翻到正面，如果是则提示通过游戏
        let total = this.row * this.column;
        if(total===this.flipCount){
            this.showModal = true;
        }
    },
    createList(){
        //生成序列，根据总牌数生成成对的序列
        let total = this.row*this.column;
        let arr = [];
        let num = 1;
        for(let i = 0;i < total;i++){
            arr.push(num);
            if((i+1)%2===0){
                num++;
            }
            // num++;
        }
        return arr;
    },
    shuffle(){
        //洗牌算法
        let arr = this.createList();

        arr.sort((a,b)=>{
            return 0.5-Math.random();
        });
        console.log(arr);
        return arr;
    },
    hideModal(){
        //关闭弹框
        this.showModal = false;
    }

  },
  mounted(){
    //重新打乱序列
    this.arrayList = this.shuffle();
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

