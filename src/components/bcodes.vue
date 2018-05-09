<template>
    <div class="bcodesMain">
      <div class="bcodes">
        <img class="bg" ref='bg' v-bind:src="bg">
        <img class='sliderImg' ref='sliderImg' v-bind:style="{left:sliderX+'px',bottom:sliderY+'px'}" v-on:load="getFix($event)" v-bind:src="sliderSrc">
      </div>
      <div class="slot">
        <div class="slott">
          <img class='slider' v-bind:style="{left:fix+sliderX+'px'}" src="../assets/icon/slider.svg" v-on:mousedown='moveDown($event)'>
          <div class="text" v-if="textShow">
            拖动滑块完成拼图以完成验证
          </div>
        </div>
      </div>
    </div>
</template>

<script>
var test = require("../assets/test.png")
export default {
  data:function(){
    return {
      bg:"http://ww2.sinaimg.cn/mw690/005zWjpngy1fr53ru83akj31hc0u01kx",
      sliderSrc:test,
      sliderX:0,
      fix:0,//滑块位置修正到对应拼图的中间
      sliderY:50,
      startX:0,
      mouseStartX:0,
      move:false, //是否开始拖动
      textShow:true
    }
  },
  props:[
    'codeConfig'
  ],
  methods:{
    moveDown(event){
      event.preventDefault()
      if(event.button === 0){//左键点击
        this.move = true
        this.mouseStartX = event.screenX;
        this.textShow = false
      }
    },
    moveUp(event){
      if(this.move){
        event.preventDefault()
        this.move = false;
        this.startX = this.sliderX;
        this.checkCode()
      }
    },
    moveIng(event){
      if(this.move){
        event.preventDefault()
        let next = this.startX + (event.screenX - this.mouseStartX)
        if(next>0&&next<this.$refs.bg.width-this.$refs.sliderImg.width){
          this.sliderX = next
        }
      }
    },
    getCode(){

    },
    checkCode(){
      fetch(this.props.codeConfig.getCode,{
        method:'POST',
        headers:this.props.codeConfig.headers,
        body:{

        }
      })
    },
    getFix(event){
      this.fix = event.target.width/2 -20
    }
  },
  mounted(){
    document.documentElement.addEventListener('mousemove',this.moveIng)
    document.documentElement.addEventListener('mouseup',this.moveUp)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.bcodesMain{
  position: relative;
  margin: 0 auto;
  display: inline-block;
}
.bcodesMain .bcodes{
  overflow: hidden;
  border: 5px solid #fff;
  border-top-right-radius: 20px;
  border-top-left-radius: 20px;
  position: relative;
}
.bcodesMain .bcodes .bg{
  max-width: 300px;
  display: block;
}
.bcodesMain .bcodes .sliderImg{
  position: absolute;
}
.bcodesMain .slot{
  border-bottom-right-radius: 20px;
  border-bottom-left-radius: 20px;
  position: absolute;
  bottom: -50px;
  height: 50px;
  width: 100%;
  background-color: #fff;
}
.bcodesMain .slot .slott{
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
  background-color: #888;
  width: calc(100% - 10px);
  border-radius: 50px;
  height: 40px;
}
.bcodesMain .slot .slott .slider{
  position: absolute;
  height: 40px;
  width: 40px;
  cursor: pointer;
}
.bcodesMain .slot .slott .text{
  text-align: center;
  line-height: 40px;
  color: #fff;
}
</style>
