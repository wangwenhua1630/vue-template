<template>
    <div  class="popup" :class="showPop?'popup-show':''">
        <div class="keyboard" :style="{background:bgcolor,fontSize:fontSize+'rpx'}">
            <div class="item btn" @click="inputNum(1)" :style="{height:itemHeight+'rpx'}">1</div>
            <div class="item btn" @click="inputNum(2)" :style="{height:itemHeight+'rpx'}">2</div>
            <div class="item btn" @click="inputNum(3)" :style="{height:itemHeight+'rpx'}">3</div>
            <div class="item btn" @click="backspace" :style="{height:itemHeight+'rpx'}"><img src="http://static.ledouya.com/FrRaeMl3SL0jLqYXJoIRbq61LgBt" /></div>
            <div class="item btn" @click="inputNum(4)" :style="{height:itemHeight+'rpx'}">4</div>
            <div class="item btn" @click="inputNum(5)" :style="{height:itemHeight+'rpx'}">5</div>
            <div class="item btn" @click="inputNum(6)" :style="{height:itemHeight+'rpx'}">6</div>
            <div class="item item-ok" :style="{background:confirmBgcolor,color:confirmTxtColor,opacity:opacityNum}" @click="confirmBtn" @touchstart="start" @touchend='end'>{{confirmTxt}}</div>
            <div class="item btn" @click="inputNum(7)" :style="{height:itemHeight+'rpx'}">7</div>
            <div class="item btn" @click="inputNum(8)" :style="{height:itemHeight+'rpx'}">8</div>
            <div class="item btn" @click="inputNum(9)" :style="{height:itemHeight+'rpx'}">9</div>
            <div class="item btn item-0" @click="inputNum(0)" :style="{height:itemHeight+'rpx'}">0</div>
            <div class="item btn" @click="inputNum('.')" :style="{height:itemHeight+'rpx'}">·</div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "keyboard",
        props: {
            //控制显示
            showPop: {
                type: Boolean,
                default: false
            },
            //背景颜色
            bgcolor: {
                type: String,
                default: "#F7F7F7"
            },
            //按钮的高度
            itemHeight:{
                type: String,
                default: "114"
            },
            //字体大小
            fontSize:{
                type: String,
                default: "50"
            },
            //确认文案
            confirmTxt:{
                type: String,
                default: "确认"
            },
            //确认按钮背景文案
            confirmBgcolor:{
                type: String,
                default: "#07C261"
            },
            //确认按钮字体颜色
            confirmTxtColor:{
                type: String,
                default: "#fff"
            }
        },
        components: {
            
        },
        computed:{

        },
        watch:{

        },
        data(){
            return {
                result:[],
                opacityNum:'1'
            }
        },
        methods: {
            //输入
          inputNum(num){
            let arr =this.result;
            if(num == '.' && arr.indexOf('.') > -1){
                return ;
            }
            arr.push(num)
            let money = arr.join('');
            this.$emit('confirm')
          },
          //退格
          backspace(){
            this.result.splice(-1,1);

            let money = this.result.join('');
            this.$emit('confirm')
          },
          //确认
          confirmBtn(){
           
          },
          start(){
              this.opacityNum  = '0.6'
          },
          end(){
              this.opacityNum  = '1'
          }
        },
        onLoad(){

        },
        onUnload(){
        }
    }
</script>

<style lang="less" scoped>
.popup{
  width:100%;
  position: fixed;
  left: 0;
  bottom: 0;
  z-index: 99999;
  visibility: hidden;
  transform: translate3d(0, 100%, 0);
  transform-origin: center;
  transition: all 0.3s ease-in-out;
}
.popup-show{
    transform: translate3d(0, 0, 0);
    visibility: visible;
}
.keyboard{
    width:100%;
    text-align:center;
    display: grid;
    grid-template-columns: repeat(4, minmax(0, 1fr));
    grid-gap: 14rpx;
    padding:15rpx;

    .item{    
        // height: 115rpx;
        background: #FFFFFF;
        border-radius: 8rpx;
        display: flex;
        justify-content: center;
        align-items: center;
        font-weight: 400;
        color: #010000;
        >img{
            width:64rpx;
            height:64rpx;
        }
    }
    .item-ok{
        height: auto;
        grid-column-start: 4;
        grid-column-end: 5;
        grid-row-start: 2;
        grid-row-end: 5;
        border-radius: 16px;
    }
    .item-0{
        grid-column-start: 1;
        grid-column-end: 3;
    }
}

.btn:active{
       background: #ccc;
}
</style>

