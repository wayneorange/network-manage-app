<template>
	<view>
		<cu-custom bgColor="bg-gradual-blue" :isBack="true"><block slot="backText">返回</block></cu-custom>
		<u-notice-bar mode="horizontal" type="primary" :list="list"></u-notice-bar>

		<!-- 时间选择器 -->
		<view class="example-body"><uni-datetime-picker type="date" :clear-icon="false" v-model="single" /></view>

	   <!-- <view>
			<picker @change="bindPickerChange" :value="index" :range="a rray">
				<view class="uni-input">{{ array[index] }}</view>
			</picker>
		</view> -->
		<view class="style"><button @click="changeOption">查询</button></view>

		<!-- eCharts折线图 -->
		<view class="content"><view id="echarts" class="echarts"></view>
		</view>

	</view>
</template>

<script module="echarts" lang="renderjs">
import request from '../../common/request.js';
var _this;
let myChart

export default {
	data() {
		return {
			list: ['当前页面为实时姿态检测历史记录'],
			
			single: '',
			datetimesingle: '',
			range: ['2021-02-1', '2021-3-28'],
			datetimerange: [],
			start: Date.now() - 1000000000,
			end: Date.now() + 1000000000,
			
			title: 'picker',
			array: ['加速度', '陀螺仪', '角度', '磁场', '温湿度', '气压和高度'],
			index: 0,
			
			UpTres: {
				title: {
					text: '历史数据',
				},
				xAxis: {
					data: [],
				},
				yAxis: {},
				series: [
					// {
					// 	name: 'transfer',
					// 	type: 'line',
					// 	data: []
					// },
					{
						name: 'signal_strenth',
						type: 'line',
						data: []
					}
				]
			}
		};
	},
	//进入网页时，自动加载的方法
	onLoad() {
		_this = this;
	},

	mounted() {
		setTimeout(() => {
			this.datetimesingle = Date.now() - 2 * 24 * 3600 * 1000;
			this.datetimerange = ['2021-07-08 0:01:10', '2021-08-08 23:59:59'];
		}, 3000);
	},

	methods: {
		change(e) {
			this.single = e;
			console.log('change事件:', e);
		},
		
		bindPickerChange: function(e) {
			console.log('picker发送选择改变，携带值为', e.detail.value);
			this.index = e.detail.value;
			this.judge(e.detail.value);
		},

		send() {
			this.getHisData();
			console.log('发送成功');	
		},

		// 根据选择的时间获取温湿度历史
		getHisData(){
			if (typeof window.echarts == 'function') {
				this.initEcharts()
			}
			else {
				// 动态引入较大类库避免影响页面展示
				const script = document.createElement('script');
				// view 层的页面运行在 www 根目录，其相对路径相对于 www 计算
				script.src = 'static/echarts.js'
				script.onload = this.initEcharts.bind(this);
				document.head.appendChild(script);
			}
		},

		initEcharts() {
			if(myChart!=null&& myChart!= ""&&myChart !=undefined){
				myChart.dispose();
			}
			myChart = echarts.init(document.getElementById('echarts'))
			let opts = {
				url: 'dz/historyData',  
				method: 'get'
			};

			var temp = { choseTime: this.single,tool: '1'};
			request.httpRequest(opts, JSON.parse(JSON.stringify(temp))).then(res => {
				console.log(res);
				console.log(res.data.length);
				uni.hideLoading();
				if (res.statusCode == 200) {
					if (res.data.length == 0) alert('没有数据');
					for (let i = 0; i < res.data.length; i++) {
						_this.UpTres.series[0].data[i] = res.data[i].signal_strength;
						console.log(this.UpTres.series[0].data[i]);
						//_this.UpTres.series[1].data[i] = res.data[i].bandwidth;
					}
					// 观测更新的数据在 view 层可以直接访问到
					myChart.setOption(_this.UpTres);
					console.log(_this.UpTres);
					_this.UpTres.series[0].data=[];
					// _this.UpTres.series[1].data=[];
				}else{
				} 
				
			});
		}
	}
};
</script>

<style>
.content {
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
}

.echarts {
	margin-top: 100px;
	width: 100%;
	height: 300px;
}

.example-body {
	/* background-color: #fff; */
	padding: 10px;
	width: 50%;
	height: 50px;
	margin-top: 10px;
	/* display: inline-block; */
}
.style{
	background-color: #fff;
	padding: 10px;
	width: 20%;
	height: 30px;
	margin-top: 10px;
	display: inline-block;
}
</style>
