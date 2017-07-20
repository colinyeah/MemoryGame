<style scoped>
.home{
    margin-top: 150px;
    text-align: center;
}
.card-block{
    display: inline-block;
    margin-right: 15px;
    margin-bottom: 15px;
}
</style>
<template>
    <div class="home">
        <!-- <div v-for="(it1,index1) in row">
            <div v-for="(it2,index2) in column"  class="card-block">
                <Card :onClickCard="clickCard" :cardId="(index1*column)+(index2+1)"></Card>
            </div>
            <br>    
        </div> -->
       <div v-for="(it1,index1) in row">
            <div v-for="(it2,index2) in column"  class="card-block" :cardId="index1*column+index2">
                <Card :onClickCard="clickCard" :dataId="arrayList[index1*column+index2]"></Card>
            </div>
            <br>    
        </div>
    </div>
</template>

<script>
import Card from './Card'
export default {
  components:{
     Card
  },
  name: 'hello',
  data () {
    return {
      row:3,
      column:4,
      arrayList:[],

      FirstCard:{},
      SecondCard:{},
      count:0,

      timeOut:null,
    }
  },
  methods:{
    clickCard(ref){
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
                this.FirstCard = {};
                this.SecondCard = {};
                this.count = 0;
            }
            
        }else{
            //当setTimeout还未执行时再次点击卡片则直接判断和后续操作，跳过等待时间
            clearTimeout(this.timeOut);
            if(this.FirstCard.dataId !== this.SecondCard.dataId){
                this.FirstCard.flip();
                this.SecondCard.flip();
            }
            // console.log(">>>",this.FirstCard,this.SecondCard);
            this.FirstCard = ref;
            this.SecondCard = {};
            this.count = 1;
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
    }

  },
  mounted(){
    //重新打乱序列
    this.arrayList = this.shuffle();
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

