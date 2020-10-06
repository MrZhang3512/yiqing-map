<template>
  <div class="hello">
    <!-- 初始化echarts需要有个宽高的盒子 -->
    <div ref="mapbox" style="height:600px;width:900px"></div>
  </div>
</template>

<script>
import echarts from 'echarts'
// 导入中国地图
import "echarts/map/js/china"
import jsonp from 'jsonp'
export default {
  data(){
    return{
      list:[],
      option:{
      title:{
        text:"疫情动态"
      },
      series:[{
        name:"当前确诊人数",
        type: 'map',
        map: 'china',
        zoom:1.1,//当前视角的缩放比例
        roam:true,//鼠标滚轮放大缩小
        label:{
          //控制对应地区的汉字
          show:true,
          color:"#333",
          fontSize:10
        },
        itemStyle:{
          //控制对应地区的样式
          areaColor:"#eee",
          // borderColor:"blue"
        },
        emphasis:{
          //控制鼠标滑过后
          label:{
          color:"#fff",
          fontSize:12
        },
        itemStyle:{
          areaColor:"#83b5e7",
        }
        },
        data:[]
        //用来展示后台传来的数据

      }],
      visualMap:[{
         type: 'piecewise', // 定义为分段型 visualMap
         splitNumber:5,//分为几段
         pieces: [
           //分段
            {min: 10000}, // 不指定 max，表示 max 为无限大（Infinity）。
            {min: 1000, max: 9999},
            {min: 100, max: 999},
            {min: 10, max: 99},
            {min: 1, max: 9}
        ],
        // align:"right"//默认为左
      }],
      tooltip:{
        trigger:'item'//鼠标移动到的区域显示数据
      },
      toolbox:{
        //左侧显示功能栏
        show: true,
        orient: 'vertical',
        left: 'right',
        top: 'center',
        feature: {
                dataView: {readOnly: false},
                restore: {},
                saveAsImage: {}
            }
      }      
    }
    }
  },
  methods:{
    //请求数据
    getData(){
      jsonp('https://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427',
            {},
            (err,data)=>{
              if(!err){
                //代表请求数据成果
                console.log(data)
                let list1=data.data.list.map(item=>({name:item.name,value:item.econNum}))
                this.option.series[0].data=list1
                 console.log(list1)
                this.e.setOption(this.option)
  
              }
            })
    }
  },
  mounted(){
    //获取数据
    this.getData()
    //初始化一个echarts
    this.e=echarts.init(this.$refs.mapbox)
    //设置option
    this.e.setOption(this.option)
  }
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
