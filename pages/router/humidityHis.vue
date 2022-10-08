<template>
	<view>
		<cu-custom bgColor="bg-gradual-blue" :isBack="true">
			<block slot="backText">返回</block>
		</cu-custom>
		<u-notice-bar mode="horizontal" type="primary" :list="list"></u-notice-bar>

		<view class="bg-custom-container">
			<!-- 时间选择器 -->
			<view class="example-body">				<view class="date-box">					<uni-datetime-picker type="date" :clear-icon="false" v-model="single" />				</view>				<u-button @click="changeOption">查询</u-button>
			</view>

			<view class="charts-item">
				<view @click="echarts.onClick" :prop="option" :change:prop="echarts.updateEcharts" id="echarts"
					class="echarts"></view>
			</view>
		</view>

	</view>
</template>

<script>
	import request from '../../common/request.js';
	export default {
		data() {
			return {
				list: ['当前页面为实时环境检测历史记录'],

				single: '',
				datetimesingle: '',
				range: ['2021-02-1', '2021-3-28'],
				datetimerange: [],
				start: Date.now() - 1000000000,
				end: Date.now() + 1000000000,

				option: {
					title: {
						text: '温湿度历史记录'
					},
					tooltip: {},
					legend: {
						data: ['temperature', 'humidity']
					},
					grid: {
						left: "3%",
						right: "4%",
						bottom: "3%",
						containLabel: true,
					},
					// 保存图表照片到本地
					toolbox: {
						feature: {
							saveAsImage: {},
						},
					},
					xAxis: {
						data: []
					},
					yAxis: {},
					series: [{
							name: 'temperature',
							type: 'line',
							data: []
						},
						{
							name: 'humidity',
							type: 'line',
							data: []
						}
					]
				}
			};
		},
		onLoad() {},

		mounted() {
			setTimeout(() => {
				this.datetimesingle = Date.now() - 2 * 24 * 3600 * 1000;
				this.datetimerange = ['2021-07-08 0:01:10', '2021-08-08 23:59:59'];
			}, 3000);
		},

		methods: {
			// async构成异步函数
			async getHisData() {
				let opts = {
					url: 'dz/getHisHumiture',
					method: 'get'
				};
				var temp = {
					choseTime: this.single
				};
				var data_1 = [];
				var data_2 = [];
				console.log(temp);
				await request.httpRequest(opts, JSON.parse(JSON.stringify(temp))).then(res => {
					console.log(res.data.length);
					if (res.statusCode == 200) {
						for (let i = 0; i < res.data.length; i++) {
							data_1[i] = res.data[i].temperature;
							data_2[i] = res.data[i].humidity;
						}
						this.option.series[0].data = data_1;
						this.option.series[1].data = data_2;
					} else {}
				});
			},

			changeOption() {
				this.option.series[0].data = [];
				this.option.series[1].data = [];
				this.getHisData();
			},
			onViewClick(options) {
				console.log(options);
			}
		}
	};
</script>

<script module="echarts" lang="renderjs">
	let myChart
	export default {
		mounted() {
			if (typeof window.echarts === 'function') {
				this.initEcharts()
			} else {
				// 动态引入较大类库避免影响页面展示
				const script = document.createElement('script')
				// view 层的页面运行在 www 根目录，其相对路径相对于 www 计算
				script.src = 'static/echarts.js'
				script.onload = this.initEcharts.bind(this)
				document.head.appendChild(script)
			}
		},
		methods: {
			initEcharts() {
				myChart = echarts.init(document.getElementById('echarts'))
				// 观测更新的数据在 view 层可以直接访问到
				myChart.setOption(this.option)
			},
			updateEcharts(newValue, oldValue, ownerInstance, instance) {
				// 监听 service 层数据变更
				myChart.setOption(newValue);
			},
			onClick(event, ownerInstance) {
				// 调用 service 层的方法
				ownerInstance.callMethod('onViewClick', {
					test: 'test'
				})
			}
		}
	}
</script>

<style lang="less">
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.echarts {
		// margin-top: 100px;
		width: 100%;
		height: 300px;
	}

	.example-body {
		/* background-color: #fff; */		display: flex;
		padding: 10px 0;		width: 100%;		margin-bottom: 20px;				.date-box {			width: 150px;		}				.u-btn {			width: 100px;			height: 35px;		}				
	}

	.style {
		width: 30%;
		margin: 10px 0;
	}

	.bg-custom-container {
		height: 100%;
		width: 100%;
		padding: 20px;
		background-color: #f1f5f9;

		.charts-item {
			width: 100%;
			height: 100%;
			border: 1px solid #FFFFFF;
			background-color: #fff;
			border-radius: 5px;
			margin-bottom: 20px;

		}

		.btns {
			margin-bottom: 20px;
			display: flex;
			width: 100%;
			align-items: center;
			justify-content: center;
		}
	}
</style>
