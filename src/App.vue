<style>
/*公共样式--开始*/
html,
body,
div,
ul,
li,
h1,
h2,
h3,
h4,
h5,
h6,
p,
dl,
dt,
dd,
ol,
form,
input,
textarea,
th,
td,
select {
  margin: 0;
  padding: 0;
}
* {
  box-sizing: border-box;
}
html,
body {
  min-height: 100%;
}

body {
  font-family: 'Microsoft YaHei';
  font-size: 14px;
  color: white;
}
h1,
h2,
h3,
h4,
h5,
h6 {
  font-weight: normal;
}
ul,
ol {
  list-style: none;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}
.leftBottom {
  width: 28%;
  height: 20%;
  margin: 20px;
  background-color: rgba(255, 255, 255, 0.2);
  position: fixed;
  bottom: 0;
}
.leftBox {
  width: 50%;
  height: 20%;
  margin: 20px;
  background-color: rgba(255, 255, 255, 0.2);
  position: fixed;
  top: 0;
  overflow-y: auto;
}
.lTitle {
  font-size: 38px;
}
.cont {
  height: 70%;
  padding: 12px 40px;
  text-align: left;
}
.ibSpan {
  display: inline-block;
  width: 48%;
}
.rightBox {
  width: 46%;
  height: 100%;
  padding: 20px;
  position: fixed;
  top: 0;
  right: 0;
  overflow-y: auto;
}
.rTitle {
  width: 70%;
  height: 30%;
  margin: auto;
  padding: 10px;
  font-size: 24px;
  background-color: rgba(255, 255, 255, 0.2);
}
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none !important;
  margin: 0;
  -moz-appearance: textfield;
}
.ipt {
  display: inline-block;
  border: 0px;
  background-color: rgba(255, 255, 255, 0.2);
  margin-right: 10px;
  color: white;
}
.chartsBox {
  height: 220px;
  margin: 10px 0;
  padding: 0px 8px 8px 8px;
  background-color: rgba(255, 255, 255, 0.2);
}
.title {
  text-align: left;
  padding: 3px 0;
}
.charts {
  height: 200px;
  background-color: rgba(255, 255, 255, 0.2);
  margin-top: -20px;
}
.anchorBL {
  display: none;
}
.loginC {
  width: 100%;
  height: 100%;
  margin: 0px;
  padding: 0px;
  background: url(../bg.jpg) no-repeat fixed;
  background-size: 100% 100%;
}
.loginBox {
  width: 20%;
  height: 40%;
  position: fixed;
  top: 20%;
  right: 15%;
  border-radius: 10px;
  background-color: rgba(255, 255, 255, 0.7);
}
.lInpt {
  width: 80%;
  height: 50px;
  margin: auto;
  border-radius: 30px;
  background-color: white;
  line-height: 50px;
}
.lBtn {
  color: white;
  font-size: 30px;
  background-color: #33adff;
  margin: 50px auto;
  cursor: pointer;
}
.iptL {
  display: inline-block;
  height: 40px;
  font-size: 18px;
  border: 0px;
  outline: none;
}
</style>
<template>
  <div id="app">
    <div class="loginC" v-if="!showM">
      <div class="loginBox">
        <p style="color:black;font-size:30px;margin:30px auto">用户登录</p>
        <div class="lInpt" style="margin-bottom:30px;">
          <input type="text" class="iptL">
        </div>
        <div class="lInpt">
          <input type="password" class="iptL">
        </div>
        <div class="lInpt lBtn" @click="login()">
          登录
        </div>
      </div>
    </div>
    <div v-if="showM" id="bigMap" style="width:100%;height:100%">
    </div>
    <div class="leftBox" v-if="showM">
      <p class="lTitle">国网线路智能监测平台</p>
      <div class="cont">
        <p>
          <span class="ibSpan">监测点数量：{{baseInfo.mPoint}}个</span>
          <span>预警监测点数量：{{baseInfo.wPoint}}个</span>
        </p>
        <p>监测内容</p>
        <p>
          <span class="ibSpan">北斗高精度监测：{{baseInfo.bPoint}}个</span>
          <span>倾斜监测：{{baseInfo.sPoint}}个</span>
        </p>
        <p>
          <span class="ibSpan">风速监测：{{baseInfo.wPoint}}个</span>
          <span>温度监测：{{baseInfo.tPoint}}个</span>
        </p>
      </div>
    </div>
    <div class="leftBottom" v-if="showM">
      <video width="100%" height="100%" autoplay="autoplay" loop="loop" controls="controls">
        <source src="http://s.grandroutes.com/av/30.mp4" type="video/mp4"> 你的浏览器暂不支持视频播放！
      </video>
    </div>
    <div class="rightBox" v-if="showM">
      <div>
        <p class="rTitle">监控数据情况</p>
        <p style="margin: 10px;">监测编号查询：
          <input type="number" v-model="id" class="ipt">
          <button @click="search()">查询</button>
        </p>
      </div>
      <div class="chartsBg">
        <div class="chartsBox">
          <p class="title">北斗高精度监测
            <button style="margin-left:10px;position: relative;z-index:999;" v-if="bChart.length>1" @click="change(1)">监测点切换</button>
          </p>
          <div class="charts" id="lh">

          </div>
        </div>
        <div class="chartsBox">
          <p class="title">倾斜监测
            <button style="margin-left:10px;position: relative;z-index:999;" v-if="sChart.length>1" @click="change(2)">监测点切换</button>
          </p>
          <div class="charts">

          </div>
        </div>
        <div class="chartsBox">
          <p class="title">风速监测</p>
          <div class="charts">

          </div>
        </div>
        <div class="chartsBox">
          <p class="title">温度监测</p>
          <div class="charts">

          </div>
        </div>
        <div class="chartsBox">
          <p class="title">湿度监测</p>
          <div class="charts">

          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
var moment = require('moment')
import DATA from '../data.js'
var icon = [
  'path://M876 456H697.5L664 288h100c15.5 0 28-12.5 28-28s-12.5-28-28-28H652.8l-24.6-123C623 82.9 600 64 573.3 64H450.7c-26.7 0-49.7 18.9-54.9 45l-24.5 123H260c-15.5 0-28 12.5-28 28s12.5 28 28 28h100l-33.5 168H148c-15.5 0-28 12.5-28 28s12.5 28 28 28h167.3l-82.8 414.9c-0.2 0.9 0.1 1.8 0 2.7-0.2 2-0.2 3.9 0.1 6 0.2 1.7 0.6 3.3 1.1 5 0.6 1.7 1.2 3.3 2.1 4.9 0.9 1.7 2 3.2 3.3 4.6 0.6 0.7 0.9 1.6 1.6 2.3 0.6 0.6 1.4 0.8 2 1.3 1.4 1.1 2.9 2 4.5 2.9 1.7 0.9 3.4 1.6 5.2 2.1 0.8 0.2 1.5 0.8 2.4 1 0.8 0.2 1.6-0.1 2.5-0.1 0.9 0.1 1.7 0.5 2.6 0.5 1 0 2-0.5 3-0.6 1.8-0.2 3.5-0.5 5.2-1 1.8-0.6 3.5-1.3 5.1-2.2 1.6-0.9 3.1-1.9 4.5-3.1 0.8-0.7 1.8-1 2.5-1.8L512 707.1l231.7 244.1c0.4 0.4 0.9 0.5 1.3 0.9 1.8 1.7 3.9 3.1 6.1 4.2 0.9 0.5 1.7 1.2 2.7 1.5 3.2 1.3 6.6 2.1 10.2 2.1 1.7 0 3.4-0.2 5.1-0.5 0.8-0.1 1.4-0.7 2.2-0.9 2-0.5 3.8-1.3 5.6-2.3 1.5-0.8 2.9-1.6 4.2-2.7 0.7-0.5 1.5-0.7 2.1-1.3 0.7-0.7 1-1.7 1.7-2.4 1.2-1.4 2.3-2.9 3.2-4.5 0.9-1.6 1.6-3.2 2.1-4.9 0.5-1.7 0.9-3.3 1.1-5 0.2-1.9 0.2-3.8 0.1-5.7-0.1-1 0.2-1.9 0-2.8L708.7 512H876c15.5 0 28-12.5 28-28s-12.5-28-28-28zM451.3 120h121.5l22.4 112H428.8l22.5-112z m-33.7 168h188.8l33.7 168H383.9l33.7-168zM620 512L512 625.8 404 512h216zM306.5 842.3L364.7 552l108.7 114.5-166.9 175.8z m352.8-290.4l58.2 290.4-166.9-175.9 108.7-114.5z'
]
export default {
  name: 'app',
  data() {
    return {
      ws: null,
      id: null,
      myChart: null,
      baseInfo: {},
      mainChart: {},
      bChart: [],
      sChart: [],
      wChart: {},
      tChart: {},
      hChart: {},
      arrUse: {
        bFirst: true,
        sFirst: true
      },
      wsParam: {},
      showM: false
    }
  },
  mounted() {},
  methods: {
    login() {
      this.showM = true
      this.formatData()
      setTimeout(() => {
        // this.websocketEvent();
        this.init()
        this.setMap()
        // this.$on('changeParam', this.onParamChange)
      }, 50)
    },
    init() {
      this.myChart = echarts.init(document.getElementById('bigMap'))
      let charts = document.getElementsByClassName('charts')
      this.eMaps = []
      for (var i = 0; i < charts.length; i++) {
        this.eMaps.push(echarts.init(charts[i]))
      }
      // charts.forEach(ele => {
      //   this.eMaps.push(echarts.init(ele));
      // });
    },
    setMap() {
      const convertData = (data, type) => {
        var res = []
        let dataList = data.slice()
        if (type === 2) {
          dataList = dataList.filter(ele => {
            return ele.isError
          })
        } else if (type === 1) {
          dataList = dataList.filter(ele => {
            return !ele.isError
          })
        }

        for (let i = 0; i < dataList.length; i++) {
          let geoCoord = this.mainChart.geoCoordMap[dataList[i].name]
          if (geoCoord) {
            res.push({
              name: dataList[i].name,
              value: geoCoord.concat([dataList[i].id, dataList[i].num]),
              symbol: type === 3 ? icon[0] : null
            })
          }
        }
        return res
      }
      var _this = this
      var options = {
        backgroundColor: '#404a59',
        title: {
          text: '',
          left: 'center',
          textStyle: {
            color: '#fff'
          }
        },
        tooltip: {
          trigger: 'item',
          formatter: function(params) {
            let num = params.data.value[2]
            let temp = `${params.data.name} <br/>
            监测点编号：${num}`
            return temp
          }
        },
        bmap: {
          center: [
            this.mainChart.center[0] + 0.14,
            this.mainChart.center[1] + 0.05
          ],
          zoom: this.mainChart.zoom, //地图放大级别
          roam: true,
          silent: true,
          mapStyle: {
            styleJson: [
              {
                featureType: 'water',
                elementType: 'all',
                stylers: {
                  color: '#021019'
                }
              },
              {
                featureType: 'highway',
                elementType: 'geometry.fill',
                stylers: {
                  color: '#000000'
                }
              },
              {
                featureType: 'highway',
                elementType: 'geometry.stroke',
                stylers: {
                  color: '#147a92'
                }
              },
              {
                featureType: 'arterial',
                elementType: 'geometry.fill',
                stylers: {
                  color: '#000000'
                }
              },
              {
                featureType: 'arterial',
                elementType: 'geometry.stroke',
                stylers: {
                  color: '#0b3d51'
                }
              },
              {
                featureType: 'local',
                elementType: 'geometry',
                stylers: {
                  color: '#000000'
                }
              },
              {
                featureType: 'land',
                elementType: 'all',
                stylers: {
                  color: '#08304b'
                }
              },
              {
                featureType: 'railway',
                elementType: 'geometry.fill',
                stylers: {
                  color: '#000000'
                }
              },
              {
                featureType: 'railway',
                elementType: 'geometry.stroke',
                stylers: {
                  color: '#08304b'
                }
              },
              {
                featureType: 'subway',
                elementType: 'geometry',
                stylers: {
                  lightness: -70
                }
              },
              {
                featureType: 'building',
                elementType: 'geometry.fill',
                stylers: {
                  color: '#000000'
                }
              },
              {
                featureType: 'all',
                elementType: 'labels.text.fill',
                stylers: {
                  color: '#857f7f'
                }
              },
              {
                featureType: 'all',
                elementType: 'labels.text.stroke',
                stylers: {
                  color: '#000000'
                }
              },
              {
                featureType: 'building',
                elementType: 'geometry',
                stylers: {
                  color: '#022338'
                }
              },
              {
                featureType: 'green',
                elementType: 'geometry',
                stylers: {
                  color: '#062032'
                }
              },
              {
                featureType: 'boundary',
                elementType: 'all',
                stylers: {
                  color: '#1e1c1c'
                }
              },
              {
                featureType: 'manmade',
                elementType: 'geometry',
                stylers: {
                  color: '#022338'
                }
              },
              {
                featureType: 'poi',
                elementType: 'all',
                stylers: {
                  visibility: 'off'
                }
              },
              {
                featureType: 'all',
                elementType: 'labels.icon',
                stylers: {
                  visibility: 'off'
                }
              },
              {
                featureType: 'all',
                elementType: 'labels.text.fill',
                stylers: {
                  color: '#2da0c6',
                  visibility: 'on'
                }
              }
            ]
          }
        },
        series: [
          {
            name: '蓝', //气球
            type: 'scatter',
            coordinateSystem: 'bmap',
            symbol: 'pin', //气泡
            symbolSize: 50,
            symbolOffset: [0, '-15%'],
            label: {
              normal: {
                show: true,
                formatter: function(params) {
                  return params.data.value[3]
                },
                textStyle: {
                  color: '#fff',
                  fontSize: 9
                }
              }
            },
            itemStyle: {
              normal: {
                color: 'blue'
                //气球颜色
              }
            },
            zlevel: 101,
            data: convertData(this.mainChart.data, 1)
          },
          {
            name: '红', //气球
            type: 'scatter',
            coordinateSystem: 'bmap',
            symbol: 'pin', //气泡
            symbolSize: 50,
            symbolOffset: [0, '-15%'],
            label: {
              normal: {
                show: true,
                formatter: function(params) {
                  return params.data.value[3]
                },
                textStyle: {
                  color: '#fff',
                  fontSize: 9
                }
              }
            },
            itemStyle: {
              normal: {
                color: 'red'
                //气球颜色
              }
            },
            zlevel: 102,
            data: convertData(this.mainChart.data, 2)
          },
          {
            name: 'Top', //底部
            type: 'scatter',
            coordinateSystem: 'bmap',
            data: convertData(this.mainChart.data, 3),
            symbolSize: function(val) {
              let size = 20
              return size
            },
            hoverAnimation: true,
            label: {
              normal: {
                formatter: '{b}',
                position: 'right',
                show: true
              }
            },
            itemStyle: {
              normal: {
                color: function(args) {
                  let color = '#fff'

                  return color
                },
                shadowBlur: 10,
                shadowColor: '#333'
              }
            },
            zlevel: 100
          }
        ]
      }
      this.myChart.setOption(options, true)
      this.myChart.on('click', function(params) {
        console.log(params)
        _this.id = params.value[2]
      })
      setTimeout(() => {
        this.bindMapListen()
      }, 500)

      let arrO = [
        {
          title: {},
          tooltip: {
            trigger: 'axis'
          },
          legend: {
            data: [
              {
                name: '东',
                icon: 'triangle'
              },
              {
                name: '北',
                icon: 'diamond'
              },
              {
                name: '天',
                icon: 'diamond'
              }
            ],
            top: 20,
            itemGap: 10,
            itemWidth: 10,
            itemHeight: 10,
            textStyle: {
              color: 'white'
            }
          },
          dataZoom: [
            {
              type: 'slider',
              show: true,
              xAxisIndex: [0],
              start: 0,
              end: 100
            },
            {
              type: 'inside',
              xAxisIndex: [0],
              start: 0,
              end: 100
            }
          ],
          xAxis: {
            type: 'category',
            boundaryGap: true, //留白
            data: this.bChart[0].xDate,
            axisLabel: {
              textStyle: {
                color: 'skyblue'
              },
              margin: 10,
              rotate: 45,
              formatter: function(value) {
                return moment(value).format('YYYY-MM-DD')
              }
            },
            axisLine: {
              onZero: false,
              lineStyle: {
                color: 'skyblue'
              }
            },
            axisTick: {
              alignWithLabel: true,
              length: 10
            }
          },
          yAxis: {
            type: 'value',
            name: '指定房屋平均偏移值（MM）',
            nameLocation: 'middle',
            nameGap: 40,
            nameTextStyle: {
              color: 'skyblue',
              fontSize: 12
            },
            axisLabel: {
              color: 'skyblue'
            },
            axisLine: {
              show: false
            },
            axisTick: {
              show: false
            }
          },
          series: [
            {
              name: '东',
              type: 'line',
              smooth: true,
              data: this.bChart[0].xData.d,
              showAllSymbol: true,
              symbol: 'triangle',
              symbolSize: 10,
              itemStyle: {
                normal: {
                  color: 'blue'
                }
              },
              lineStyle: {
                normal: {
                  width: 0
                }
              }
            },
            {
              name: '北',
              type: 'line',
              smooth: true,
              data: this.bChart[0].xData.b,
              showAllSymbol: true,
              symbol: 'diamond',
              symbolSize: 10,
              itemStyle: {
                normal: {
                  color: 'orange'
                }
              },
              lineStyle: {
                normal: {
                  width: 0
                }
              }
            },
            {
              name: '天',
              type: 'line',
              smooth: true,
              data: this.bChart[0].xData.t,
              showAllSymbol: true,
              symbol: 'diamond',
              symbolSize: 10,
              itemStyle: {
                normal: {
                  color: 'pink'
                }
              },
              lineStyle: {
                normal: {
                  width: 0
                }
              }
            }
          ]
        },
        {
          title: {},
          tooltip: {
            trigger: 'axis'
          },
          legend: {
            data: [
              {
                name: 'X',
                icon: 'diamond'
              },
              {
                name: 'Y',
                icon: 'triangle'
              }
            ],
            top: 20,
            itemGap: 10,
            itemWidth: 10,
            itemHeight: 10,
            textStyle: {
              color: 'white'
            }
          },
          dataZoom: [
            {
              type: 'slider',
              show: true,
              xAxisIndex: [0],
              start: 0,
              end: 100
            },
            {
              type: 'inside',
              xAxisIndex: [0],
              start: 0,
              end: 100
            }
          ],
          xAxis: {
            type: 'category',
            boundaryGap: true,
            data: this.sChart[0].xDate,
            axisLabel: {
              textStyle: {
                color: 'skyblue'
              },
              margin: 10,
              rotate: 45,
              formatter: function(value) {
                return moment(value).format('YYYY-MM-DD')
              }
            },
            axisLine: {
              onZero: false,
              lineStyle: {
                color: 'skyblue'
              }
            },
            axisTick: {
              alignWithLabel: true,
              length: 10
            }
          },
          yAxis: {
            type: 'value',
            name: '房屋倾斜率变化',
            nameLocation: 'middle',
            nameGap: 40,
            nameTextStyle: {
              color: 'skyblue',
              fontSize: 12
            },
            axisLabel: {
              color: 'skyblue'
            },
            axisLine: {
              show: false
            },
            axisTick: {
              show: false
            }
          },
          series: [
            {
              name: 'X',
              type: 'line',
              smooth: true,
              data: this.sChart[0].xData.x,
              showAllSymbol: true,
              symbol: 'diamond',
              symbolSize: 10,
              itemStyle: {
                normal: {
                  color: 'blue'
                }
              },
              lineStyle: {
                normal: {
                  width: 0
                }
              }
            },
            {
              name: 'Y',
              type: 'line',
              smooth: true,
              data: this.sChart[0].xData.y,
              showAllSymbol: true,
              symbol: 'triangle',
              symbolSize: 10,
              itemStyle: {
                normal: {
                  color: 'orange'
                }
              },
              lineStyle: {
                normal: {
                  width: 0
                }
              }
            }
          ]
        },
        {
          title: {},
          tooltip: {
            trigger: 'axis'
          },
          dataZoom: [
            {
              type: 'slider',
              show: true,
              xAxisIndex: [0],
              start: 0,
              end: 100
            },
            {
              type: 'inside',
              xAxisIndex: [0],
              start: 0,
              end: 100
            }
          ],
          xAxis: {
            type: 'category',
            boundaryGap: false,
            data: this.wChart.xDate,
            axisLabel: {
              textStyle: {
                color: 'skyblue'
              },
              margin: 10,
              rotate: 45,
              formatter: function(value) {
                return moment(value).format('YYYY-MM-DD')
              }
            },
            axisLine: {
              onZero: false,
              lineStyle: {
                color: 'skyblue'
              }
            },
            axisTick: {
              alignWithLabel: true,
              length: 10
            }
          },
          yAxis: {
            type: 'value',
            name: '裂缝宽度变化(mm)',
            nameLocation: 'middle',
            nameGap: 40,
            nameTextStyle: {
              color: 'skyblue',
              fontSize: 12
            },
            axisLabel: {
              color: 'skyblue'
            },
            axisLine: {
              show: false
            },
            axisTick: {
              show: false
            }
          },
          series: [
            {
              name: '风速数据',
              type: 'line',
              smooth: true,
              data: this.wChart.xData.d,
              showAllSymbol: true,
              symbol: 'triangle',
              symbolSize: 10,
              lineStyle: {
                normal: {
                  color: 'blue',
                  width: 0,
                  type: 'dotted'
                }
              }
            }
          ]
        },
        {
          title: {},
          tooltip: {
            trigger: 'axis'
          },
          dataZoom: [
            {
              type: 'slider',
              show: true,
              xAxisIndex: [0],
              start: 0,
              end: 100
            },
            {
              type: 'inside',
              xAxisIndex: [0],
              start: 0,
              end: 100
            }
          ],
          xAxis: {
            type: 'category',
            boundaryGap: false,
            data: this.tChart.xDate,
            axisLabel: {
              textStyle: {
                color: 'skyblue'
              },
              margin: 10,
              rotate: 45,
              formatter: function(value) {
                return moment(value).format('YYYY-MM-DD')
              }
            },
            axisLine: {
              onZero: false,
              lineStyle: {
                color: 'skyblue'
              }
            },
            axisTick: {
              alignWithLabel: true,
              length: 10
            }
          },
          yAxis: {
            type: 'value',
            name: '裂缝宽度变化(mm)',
            nameLocation: 'middle',
            nameGap: 40,
            nameTextStyle: {
              color: 'skyblue',
              fontSize: 12
            },
            axisLabel: {
              color: 'skyblue'
            },
            axisLine: {
              show: false
            },
            axisTick: {
              show: false
            }
          },
          series: [
            {
              name: '温度数据',
              type: 'line',
              smooth: true,
              data: this.tChart.xData.d,
              showAllSymbol: true,
              symbol: 'triangle',
              symbolSize: 10,
              lineStyle: {
                normal: {
                  color: 'blue',
                  width: 0,
                  type: 'dotted'
                }
              }
            }
          ]
        },
        {
          title: {},
          tooltip: {
            trigger: 'axis'
          },
          dataZoom: [
            {
              type: 'slider',
              show: true,
              xAxisIndex: [0],
              start: 0,
              end: 100
            },
            {
              type: 'inside',
              xAxisIndex: [0],
              start: 0,
              end: 100
            }
          ],
          xAxis: {
            type: 'category',
            boundaryGap: false,
            data: this.hChart.xDate,
            axisLabel: {
              textStyle: {
                color: 'skyblue'
              },
              margin: 10,
              rotate: 45,
              formatter: function(value) {
                return moment(value).format('YYYY-MM-DD')
              }
            },
            axisLine: {
              onZero: false,
              lineStyle: {
                color: 'skyblue'
              }
            },
            axisTick: {
              alignWithLabel: true,
              length: 10
            }
          },
          yAxis: {
            type: 'value',
            name: '裂缝宽度变化(mm)',
            nameLocation: 'middle',
            nameGap: 40,
            nameTextStyle: {
              color: 'skyblue',
              fontSize: 12
            },
            axisLabel: {
              color: 'skyblue'
            },
            axisLine: {
              show: false
            },
            axisTick: {
              show: false
            }
          },
          series: [
            {
              name: '湿度数据',
              type: 'line',
              smooth: true,
              data: this.hChart.xData.d,
              showAllSymbol: true,
              symbol: 'triangle',
              symbolSize: 10,
              lineStyle: {
                normal: {
                  color: 'blue',
                  width: 0,
                  type: 'dotted'
                }
              }
            }
          ]
        }
      ]
      this.eMaps.forEach((ele, idx) => {
        ele.setOption(arrO[idx], true)
      })
    },
    bindMapListen() {
      var bmap = this.myChart
        .getModel()
        .getComponent('bmap')
        .getBMap()
      bmap.addControl(new BMap.MapTypeControl())
      bmap.addControl(new BMap.ScaleControl())

      bmap.enableAutoResize()
      bmap.addEventListener('zoomend', (type, target) => {
        this.mainChart.zoom = bmap.getZoom()
        this.mainChart.center = [bmap.getCenter().lng, bmap.getCenter().lat]
        this.$emit('changeParam', { mapZoom: this.mainChart.zoom })
      })
      bmap.addEventListener('click', function(type) {
        return false
      })
    },
    search() {
      console.log(this.id)
      // this.setMap()
    },
    change(type) {
      let dataArr = type === 1 ? this.bChart : this.sChart
      let isFirst = type === 1 ? this.arrUse.bFirst : this.arrUse.sFirst
      let obj = isFirst ? dataArr[1] : dataArr[0]
      this.arrUse.bFirst = type === 1 ? !this.arrUse.bFirst : this.arrUse.bFirst
      this.arrUse.sFirst = type === 2 ? !this.arrUse.sFirst : this.arrUse.sFirst
      let option =
        type === 1
          ? {
              title: {},
              tooltip: {
                trigger: 'axis'
              },
              legend: {
                data: [
                  {
                    name: '东',
                    icon: 'triangle'
                  },
                  {
                    name: '北',
                    icon: 'diamond'
                  },
                  {
                    name: '天',
                    icon: 'diamond'
                  }
                ],
                top: 20,
                itemGap: 10,
                itemWidth: 10,
                itemHeight: 10,
                textStyle: {
                  color: 'white'
                }
              },
              dataZoom: [
                {
                  type: 'slider',
                  show: true,
                  xAxisIndex: [0],
                  start: 0,
                  end: 100
                },
                {
                  type: 'inside',
                  xAxisIndex: [0],
                  start: 0,
                  end: 100
                }
              ],
              xAxis: {
                type: 'category',
                boundaryGap: true, //留白
                data: obj.xDate,
                axisLabel: {
                  textStyle: {
                    color: 'skyblue'
                  },
                  margin: 10,
                  rotate: 45,
                  formatter: function(value) {
                    return moment(value).format('YYYY-MM-DD')
                  }
                },
                axisLine: {
                  onZero: false,
                  lineStyle: {
                    color: 'skyblue'
                  }
                },
                axisTick: {
                  alignWithLabel: true,
                  length: 10
                }
              },
              yAxis: {
                type: 'value',
                name: '指定房屋平均偏移值（MM）',
                nameLocation: 'middle',
                nameGap: 40,
                nameTextStyle: {
                  color: 'skyblue',
                  fontSize: 12
                },
                axisLabel: {
                  color: 'skyblue'
                },
                axisLine: {
                  show: false
                },
                axisTick: {
                  show: false
                }
              },
              series: [
                {
                  name: '东',
                  type: 'line',
                  smooth: true,
                  data: obj.xData.d,
                  showAllSymbol: true,
                  symbol: 'triangle',
                  symbolSize: 10,
                  itemStyle: {
                    normal: {
                      color: 'blue'
                    }
                  },
                  lineStyle: {
                    normal: {
                      width: 0
                    }
                  }
                },
                {
                  name: '北',
                  type: 'line',
                  smooth: true,
                  data: obj.xData.b,
                  showAllSymbol: true,
                  symbol: 'diamond',
                  symbolSize: 10,
                  itemStyle: {
                    normal: {
                      color: 'orange'
                    }
                  },
                  lineStyle: {
                    normal: {
                      width: 0
                    }
                  }
                },
                {
                  name: '天',
                  type: 'line',
                  smooth: true,
                  data: obj.xData.t,
                  showAllSymbol: true,
                  symbol: 'diamond',
                  symbolSize: 10,
                  itemStyle: {
                    normal: {
                      color: 'pink'
                    }
                  },
                  lineStyle: {
                    normal: {
                      width: 0
                    }
                  }
                }
              ]
            }
          : {
              title: {},
              tooltip: {
                trigger: 'axis'
              },
              legend: {
                data: [
                  {
                    name: 'X',
                    icon: 'diamond'
                  },
                  {
                    name: 'Y',
                    icon: 'triangle'
                  }
                ],
                top: 20,
                itemGap: 10,
                itemWidth: 10,
                itemHeight: 10,
                textStyle: {
                  color: 'white'
                }
              },
              dataZoom: [
                {
                  type: 'slider',
                  show: true,
                  xAxisIndex: [0],
                  start: 0,
                  end: 100
                },
                {
                  type: 'inside',
                  xAxisIndex: [0],
                  start: 0,
                  end: 100
                }
              ],
              xAxis: {
                type: 'category',
                boundaryGap: true,
                data: obj.xDate,
                axisLabel: {
                  textStyle: {
                    color: 'skyblue'
                  },
                  margin: 10,
                  rotate: 45,
                  formatter: function(value) {
                    return moment(value).format('YYYY-MM-DD')
                  }
                },
                axisLine: {
                  onZero: false,
                  lineStyle: {
                    color: 'skyblue'
                  }
                },
                axisTick: {
                  alignWithLabel: true,
                  length: 10
                }
              },
              yAxis: {
                type: 'value',
                name: '房屋倾斜率变化',
                nameLocation: 'middle',
                nameGap: 40,
                nameTextStyle: {
                  color: 'skyblue',
                  fontSize: 12
                },
                axisLabel: {
                  color: 'skyblue'
                },
                axisLine: {
                  show: false
                },
                axisTick: {
                  show: false
                }
              },
              series: [
                {
                  name: 'X',
                  type: 'line',
                  smooth: true,
                  data: obj.xData.x,
                  showAllSymbol: true,
                  symbol: 'diamond',
                  symbolSize: 10,
                  itemStyle: {
                    normal: {
                      color: 'blue'
                    }
                  },
                  lineStyle: {
                    normal: {
                      width: 0
                    }
                  }
                },
                {
                  name: 'Y',
                  type: 'line',
                  smooth: true,
                  data: obj.xData.y,
                  showAllSymbol: true,
                  symbol: 'triangle',
                  symbolSize: 10,
                  itemStyle: {
                    normal: {
                      color: 'orange'
                    }
                  },
                  lineStyle: {
                    normal: {
                      width: 0
                    }
                  }
                }
              ]
            }

      this.eMaps[type - 1].setOption(option, true)
    },
    websocketEvent() {
      this.ws = new WebSocket('wss://echo.websocket.org')
      this.ws.onopen = evt => {
        this.ws.send(JSON.stringify(this.wsParam))
        // setInterval(() => {
        //   this.ws.send("北京时间：" + moment().format("YY-HH-dd hh:mm:ss"));
        // }, 1000);
      }
      this.ws.onmessage = evt => {
        console.log(evt.data)
        // this.formatData(eval('(' + evt.data + ')'))
        this.formatData(data) //evt.data
      }

      this.ws.onclose = evt => {}
    },
    formatData(data) {
      // let { baseInfo, mainChart, bChart, sChart, wChart, tChart, hChart } = data
      let { baseInfo, mainChart, bChart, sChart, wChart, tChart, hChart } = DATA
      this.baseInfo = baseInfo
      this.mainChart = mainChart
      this.bChart = bChart
      this.sChart = sChart
      this.wChart = wChart
      this.tChart = tChart
      this.hChart = hChart
      // this.setMap()
    },
    onParamChange(data) {
      this.wsParam = Object.assign(this.wsParam, data)
      this.ws.send(JSON.stringify(this.wsParam))
    }
  }
}
</script>


