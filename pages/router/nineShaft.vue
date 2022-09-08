<template>
	<view>
		<cu-custom bgColor='bg-gradual-blue':isBack='true'>
			<block slot='backText'>返回</block>
		</cu-custom>
		
		<view class="cu-bar bg-white margin-top-xs">	
			<view class="text">
				<text class="text-xl text-bold text-blue text-shadow">磁场</text>
			</view>
		</view>
		<!-- 磁场 -->
		  <view class="charts-box">
		    <qiun-data-charts 
		      type="line"
		      :opts="opts"
		      :chartData="chartData"
		    />
		  </view>
		  
		<view class="cu-bar bg-white margin-top-xs">
			<view class="text">
				<text class="text-xl text-bold text-blue text-shadow">加速度</text>
			</view>
		</view>  
		 <!-- 加速度 -->
		<view class="charts-box">
		  <qiun-data-charts 
		    type="line"
		    :opts="Sopts"
		    :chartData="SchartData"
		  />
		</view>		
		<!-- 陀螺仪 -->
		<view class="cu-bar bg-white margin-top-xs">
			<view class="text">
				<text class="text-xl text-bold text-blue text-shadow">陀螺仪</text>
			</view>
		</view> 
		
		<view class="charts-box">
		  <qiun-data-charts 
		    type="line"
		    :opts="Gopts"
		    :chartData="GchartData"
		  />
		</view>	
		<!-- 角度 -->
		<view class="cu-bar bg-white margin-top-xs">
			<view class="text">
				<text class="text-xl text-bold text-blue text-shadow">角度</text>
			</view>
		</view> 
		
		<view class="charts-box">
		  <qiun-data-charts 
		    type="line"
		    :opts="Aopts"
		    :chartData="AchartData"/>
		</view>	
		
		
		
		<view class="cu-bar bg-white margin-top-xs">	
			<view class="text" >
				<text class="text-xl text-bold text-blue text-shadow">温度和湿度</text>
			</view>
		</view>	
		<!-- 温度和湿度 -->
			  <view>
			    <canvas canvas-id="myid" id="myid" class="charts" @tap="tap"></canvas>
			  </view>
			
	</view>
</template>

<script>
	import uCharts from '../../components/u-charts/u-charts.js';
	import request from '../../common/request.js';
	var uChartsInstance = {};
	var _this;
	var Temperature;
	var Humidity;
export default {
  data() {
    return {
		 Temperature:null,
		 Humidity:null,
		 timer: null,
		 cWidth: 750,
		 cHeight: 500,
		 SchartData:{},
		 GchartData:{},
		 AchartData:{},
      chartData: {},
      //您可以通过修改 config-ucharts.js 文件中下标为 ['line'] 的节点来配置全局默认参数，如都是默认参数，此处可以不传 opts 。实际应用过程中 opts 只需传入与全局默认参数中不一致的【某一个属性】即可实现同类型的图表显示不同的样式，达到页面简洁的需求。
      opts: {
        color: ["#1890FF","#91CB74","#FAC858","#EE6666","#73C0DE","#3CA272","#FC8452","#9A60B4","#ea7ccc"],
        padding: [15,10,0,15],
        legend: {},
        xAxis: {
          disableGrid: true
        },
        yAxis: {
          gridType: "dash",
          dashLength: 2
        },
        extra: {
          line: {
            type: "straight",
            width: 2
          }
        }
		},
		Sopts: {
		  color: ["#1890FF","#91CB74","#FAC858","#EE6666","#73C0DE","#3CA272","#FC8452","#9A60B4","#ea7ccc"],
		  padding: [15,10,0,15],
		  legend: {},
		  xAxis: {
		    disableGrid: true
		  },
		  yAxis: {
		    gridType: "dash",
		    dashLength: 2,
			data:[
			  {
			    "min":-1,
			    "max":12,
			  }
			]
		  },
		  extra: {
		    line: {
		      type: "straight",
		      width: 2
		    }
		  }
		},
		Gopts: {
		  color: ["#1890FF","#91CB74","#FAC858","#EE6666","#73C0DE","#3CA272","#FC8452","#9A60B4","#ea7ccc"],
		  padding: [15,10,0,15],
		  legend: {},
		  xAxis: {
		    disableGrid: true
		  },
		  yAxis: {
		    gridType: "dash",
		    dashLength: 2,
			data:[
			  {
			    "min":-180,
			    "max":180,
			  }
			]
		  },
		  extra: {
		    line: {
		      type: "straight",
		      width: 2
		    }
		  }
		},
		
		Aopts: {
		  color: ["#1890FF","#91CB74","#FAC858","#EE6666","#73C0DE","#3CA272","#FC8452","#9A60B4","#ea7ccc"],
		  padding: [15,10,0,15],
		  legend: {},
		  xAxis: {
		    disableGrid: true
		  },
		  yAxis: {
		    gridType: "dash",
		    dashLength: 2,
			data:[
			  {
			    "min":-180,
			    "max":180,
			  }
			]
		  },
		  extra: {
		    line: {
		      type: "straight",
		      width: 2
		    }
		  }
		},
		
		UpTres:{
		    categories: ["","","","","",""],
		    series: [
					{ 
					  name: "temperature",	
						data: [],
					},
					{
					  name: "humidity",
					  data: [],	
					}
		    ],
		},
		Upres:{
		    categories: ["","","","","",""],
		    series: [
		      {
		        name: "MagneticX",
		        data: []
		      },
		      {
		        name: "MagneticY",
		        data: []
		      },
		      {
		        name: "MagneticZ",
		        data: []
		      }
		    ]
		},
		Supres:{
		    categories: ["","","","","",""],
		    series: [
		      {
		        name: "AccX",
		        data: []
		      },
		      {
		        name: "AccY",
		        data: []
		      },
		      {
		        name: "AccZ",
		        data: []
		      }
		    ]
		},
		Gupres:{
		    categories: ["","","","","",""],
		    series: [
		      {
		        name: "Gyro_X",
		        data: []
		      },
		      {
		        name: "Gyro_Y",
		        data: []
		      },
		      {
		        name: "Gyro_Z",
		        data: []
		      }
		    ]
		},
		Aupres:{
		    categories: ["","","","","",""],
		    series: [
		      {
		        name: "Angle_X",
		        data: []
		      },
		      {
		        name: "Angle_Y",
		        data: []
		      },
		      {
		        name: "Angle_Z",
		        data: []
		      }
		    ]
		},
	};
},			
  onReady() {
	_this=this; 
	//这里的 750 对应 css .charts 的 width
	this.cWidth = uni.upx2px(750);
	//这里的 500 对应 css .charts 的 height
	this.cHeight = uni.upx2px(500);
    this.getServerData();
  },
  mounted() {
  	this.time();
  },
  methods: {
	time() {
	  	_this.timer = setInterval(() => {
	  		this.getData()
	  	}, 5000);
		},
	getData() {
		console.log('数据加载');
		let opts = {
			url: 'dz/getHumiture',
			method: 'get'
		};
		let Nineopts = {
			url: 'dz/getNineShaft',
			method: 'get'
		};
		let Speedopts= {
			url: 'dz/getNineShaft',
			method: 'get'
		};
		let Gopts= {
			url: 'dz/getNineShaft',
			method: 'get'
		};
		let Aopts={
			url: 'dz/getNineShaft',
			method: 'get'
		};
		request.httpRequest(opts).then(res => {
				
		  		console.log(res);
				Temperature=JSON.parse(JSON.stringify(res.data.temperature));
				Humidity=JSON.parse(JSON.stringify(res.data.humidity));
				console.log("当前后台温度数据是："+res.data.temperature);
		  		if (res.statusCode == 200) {
					
		  			for(let i=0;i<=5;i++)
		  				{
		  					_this.UpTres.series[0].data[i]=Temperature;
							_this.UpTres.series[1].data[i]=Humidity;
		  					console.log("温度："+_this.UpTres.series[0].data[i]);
							console.log("湿度："+_this.UpTres.series[1].data[i]);
		  					
		  				}
					console.log(_this.UpTres.series[0].data);
					console.log(_this.UpTres.series[1].data);
					_this.updateCharts("myid",_this.UpTres);
		  		} else {}
		  	});
		request.httpRequest(Nineopts).then(res => {
		  		console.log(res);
		  		uni.hideLoading();
		  		if (res.statusCode == 200) {
					
					for(let i=0;i<=5;i++)
						{
							_this.Upres.series[0].data[i]=res.data.magnetic_x;
							_this.Upres.series[1].data[i]=res.data.magnetic_y;
							_this.Upres.series[2].data[i]=res.data.magnetic_z;
						}
					 _this.chartData = JSON.parse(JSON.stringify(_this.Upres));
					 console.log(_this.Upres.series[0].data);
		  		} else {}
		  	});	
		request.httpRequest(Speedopts).then(res => {
		  		console.log(res);
		  		uni.hideLoading();
		  		if (res.statusCode == 200) {
					
					for(let i=0;i<=5;i++)
						{
							_this.Supres.series[0].data[i]=res.data.acc_x;
							_this.Supres.series[1].data[i]=res.data.acc_y;
							_this.Supres.series[2].data[i]=res.data.acc_z;
						}
					 _this.SchartData = JSON.parse(JSON.stringify(_this.Supres));
					 console.log(_this.Supres.series[0].data);
		  		} else {}
		  	});	
		request.httpRequest(Gopts).then(res => {
		  		console.log(res);
		  		uni.hideLoading();
		  		if (res.statusCode == 200) {
					
					for(let i=0;i<=5;i++)
						{
							_this.Gupres.series[0].data[i]=res.data.gyro_x;
							_this.Gupres.series[1].data[i]=res.data.gyro_y;
							_this.Gupres.series[2].data[i]=res.data.gyro_z;
						}
					 _this.GchartData = JSON.parse(JSON.stringify(_this.Gupres));
					 console.log(_this.Gupres.series[0].data);
		  		} else {}
		  	});
		request.httpRequest(Aopts).then(res => {
		  		console.log(res);
		  		uni.hideLoading();
		  		if (res.statusCode == 200) {
					
					for(let i=0;i<=5;i++)
						{
							_this.Aupres.series[0].data[i]=res.data.angle_x;
							_this.Aupres.series[1].data[i]=res.data.angle_y;
							_this.Aupres.series[2].data[i]=res.data.angle_z;
						}
					 _this.AchartData = JSON.parse(JSON.stringify(_this.Aupres));
					 console.log(_this.Aupres.series[0].data);
		  		} else {}
		  	});								
		},
    getServerData() {
      //模拟从服务器获取数据时的延时
      setTimeout(() => {
        //模拟服务器返回数据，如果数据格式和标准格式不同，需自行按下面的格式拼接
        let res = {
            categories: ["","","","","",""],
            series: [
              {
                name: "MagneticX",
                data: [0,0,0,0,0,0]
              },
              {
                name: "MagneticY",
                data: [0,0,0,0,0,0]
              },
              {
                name: "MagneticZ",
                data: [0,0,0,0,0,0]
              }
            ]
          };
		 let Tres={
		     categories: ["","","","","",""],
		     series: [
		       {
		         name: "temperature",
		         data: [0,0,0,0,0,0],
		       },
		 	  {
		 		name: "humidity",
		 		data: [0,0,0,0,0,0],  
		 	  }
		     ],
		 };
		let Sres={
		    categories: ["","","","","",""],
		    series: [
		      {
		        name: "AccX",
		        data:  [0,0,0,0,0,0]
		      },
		      {
		        name: "AccY",
		        data:  [0,0,0,0,0,0]
		      },
		      {
		        name: "AccZ",
		        data:  [0,0,0,0,0,0]
		      }
		    ]
		};
		let Gres={
		    categories: ["","","","","",""],
		    series: [
		      {
		        name: "Gyro_X",
		        data: [0,0,0,0,0,0]
		      },
		      {
		        name: "Gyro_Y",
		        data: [0,0,0,0,0,0]
		      },
		      {
		        name: "Gyro_Z",
		        data: [0,0,0,0,0,0]
		      }
		    ]
		};
		let Ares={
		    categories: ["","","","","",""],
		   series: [
		     {
		       name: "Angle_X",
		       data: [0,0,0,0,0,0]
		     },
		     {
		       name: "Angle_Y",
		       data: [0,0,0,0,0,0]
		     },
		     {
		       name: "Angle_Z",
		       data: [0,0,0,0,0,0]
		     }
		   ]
		};
		this.AchartData=JSON.parse(JSON.stringify(Ares));
		this.GchartData= JSON.parse(JSON.stringify(Gres));
		this.SchartData= JSON.parse(JSON.stringify(Sres));
        this.chartData = JSON.parse(JSON.stringify(res));
		this.drawCharts('myid', Tres);
      }, 500);
    },
	drawCharts(id,data){
	      const ctx = uni.createCanvasContext(id, this);
	      uChartsInstance[id] = new uCharts({
	        type: "line",
	        context: ctx,
	        width: this.cWidth,
	        height: this.cHeight,
	        categories: data.categories,
	        series: data.series,
	        xAxis: {
	          disableGrid: true
	        },
	        yAxis: {
	          data: [ { min: 0 ,
						max:100,
			  } ]
	        },
	        extra: {
	          column: {
	            type: "group"
	          }
	        }
	      });
	    },
	tap(e) {
		console.log(e);
	    //格式化ToolTip
	    uChartsInstance[e.target.id].showToolTip(e, {
	        formatter: (item, category, index, opts) => {
	            return item.name + ":" + item.data;
	        }
	    });	
	},		
	updateCharts(id,data) {
	  uChartsInstance[id].updateData({
	    series: data.series,
	    animation: false
	  });
	},
},
	beforeDestroy() {
		if (_this.timer) { //如果定时器还在运行 或者直接关闭，不用判断
			clearInterval(_this.timer); //关闭
		}
	},	
};


</script>

<style scoped>
  /* 请根据实际需求修改父元素尺寸，组件自动识别宽高 */
  .charts-box {
    width: 100%;
    height: 500rpx;
  }	
  .text{
		margin-left: 20px;
	}
	.charts{
	    width: 750rpx;
	    height: 500rpx;
	  }	
</style>


