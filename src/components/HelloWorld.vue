<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <input @click="routate" class="rot" type="button" value="旋转">
    <canvas class="draw" width="600" height="600" ref="canvas">
    </canvas>
  </div>
</template>

<script>
import { setTimeout, clearTimeout, clearInterval } from 'timers';
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data() {
    return {
      context: undefined,
      canvasW:undefined,
      canvasH:undefined,
      time: 100,
    };
  },
  mounted() {
    const canvas = this.$refs.canvas;
    this.context = canvas.getContext("2d");
    this.context.strokeStyle = "white";
    this.context.font = '26px Arail bold';
    this.canvasW = canvas.width; // 画板的高度
    this.canvasH = canvas.height; // 画板的宽度
    // 控制参数
    let colors = ["red", "blue", "black", "orange"]; //颜色
    let prize = ["一等奖", "二等奖", "三等奖", "四等奖"];//奖项名字
    let num = 4;
    this.init(colors,num,prize);
  },
  methods: {
    init(colors,num,prize){
      //计算每个奖项所占角度数
      let baseAngle = Math.PI * 2 / num;
      // 背景色
      for(let index = 0; index < num; index++) {
        this.context.beginPath();
        let angle = index * baseAngle;
        this.context.fillText(prize[index],Math.cos(angle)*200,Math.sin(angle)*200)
        this.context.arc(this.canvasW * 0.5, this.canvasH * 0.5, this.canvasH/2, angle-baseAngle/2, angle + baseAngle/2,false);
        this.context.beginPath();
        this.context.arc(this.canvasW * 0.5, this.canvasH * 0.5, this.canvasH/12, angle + baseAngle/2, angle-baseAngle/2, false);

        this.context.fillStyle = colors[index]; //设置每个扇形区域的颜色
        this.context.fill(); //填充颜色
        this.context.save(); //保存当前环境的状态
      } 
      // 画箭头
      this.context.beginPath();
      this.context.moveTo(this.canvasW * 0.5-this.canvasH/12, this.canvasH * 0.5);
      let height = this.canvasH/6*Math.sin(Math.PI/3);//计算等边三角形的高
      this.context.lineTo(this.canvasW * 0.5-this.canvasH/12,this.canvasH * 0.5);
      this.context.lineTo(this.canvasW * 0.5,this.canvasH * 0.5-height);
      this.context.lineTo(this.canvasW * 0.5+this.canvasH/12,this.canvasH * 0.5);
      this.context.rotate(1)
      this.context.fillStyle='white'; //以上面定义的渐变填充
      this.context.fill(); //闭合形状并且以填充方式绘制出来
      this.context.save(); //保存当前环境的状态

    },
    routate(){

      // clearInterval(routate);
      // let numT = Math.random()*100;
      // let num = 0;
      // var setT = setInterval(()=>{
      //     console.log(num,numT)
      //     num+=10;
      //     this.context.rotate(100*Math.PI/180)
      //   if(num>numT){
      //     console.log(num,numT)
      //     clearInterval(setT)
      //   }
      // },120)
      
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.draw {
  width: 500px;
  height: 500px;
  border: 10px solid yellowgreen;
  border-radius: 50%;
  margin: 20px auto;
}
.rot{
  width: 80px;
  height: 40px;
}
</style>
