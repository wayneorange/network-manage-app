<template>
	<view>
		<cu-custom bgColor="bg-gradual-blue" :isBack="true">
			<block slot="backText">返回</block>
		</cu-custom>
		<u-notice-bar mode="horizontal" type="primary" :list="list"></u-notice-bar>

		<view class="bg-custom-container">
			<!-- 按钮 -->
			<view class="btns">
				<button size="mini" class="Starttest" plain="true" @click="startTest">开始测试</button>
				<button size="mini" class="Stoptest" plain="true" @click="stopTest">停止测试</button>
			</view>

			<view class="charts-item">
				<!-- 温度 -->
				<view class="cu-bar bg-white margin-top-xs">
					<view class="action sub-title">
						<text class="text-xl text-bold text-blue text-shadow">温度盘</text>
						<text class="text-ABC text-blue" style="white-space: nowrap;">温度盘</text>
					</view>
				</view>
				<view class="chartsMain"><canvas canvas-id="canvasGauge" id="canvasGauge" class="charts"></canvas>
				</view>
			</view>

			<view class="charts-item">
				<!-- 湿度 -->
				<view class="cu-bar bg-white margin-top-xs">
					<view class="action sub-title">
						<text class="text-xl text-bold text-blue text-shadow">湿度图</text>
						<text class="text-ABC text-blue" style="white-space: nowrap;">湿度图</text>
					</view>
				</view>
				<view class="chartsMain"><canvas canvas-id="canvasHumidity" id="canvasHumidity" class="charts"></canvas>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	// 图表
	import uCharts from '@/components/u-charts/u-charts.js';
	import request from '@/common/request.js';
	var _self;
	var canvaGauge = null;
	var canvasHumidity = null;

	export default {
		data() {
			return {
				list: ['图表目前均使用：《uCharts高性能跨端图表》，小程序如需接入Echarts等其他图表，敬请期待作者更新...'],
				timer: null,
				cWidth: '',
				cHeight: '',
				pixelRatio: 1,
				// 仪表盘
				gaugeWidth: 15,
				chartData: {
					categories: [{
							value: 0.2,
							color: '#2fc25b'
						},
						{
							value: 0.8,
							color: '#f04864'
						},
						{
							value: 1,
							color: '#1890ff'
						}
					],
					series: [{
						name: '温度()',
						data: 0.85
					}]
				},
				humidity: {
					categories: [{
							value: 20,
							color: '#2fc25b'
						},
						{
							value: 80,
							color: '#f04864'
						},
						{
							value: 100,
							color: '#1890ff'
						}
					],
					series: [{
						name: '湿度()',
						data: 0.7
					}]
				},
				projectList: []
			};
		},
		onLoad() {
			_self = this;
			this.cWidth = uni.upx2px(750);
			this.cHeight = uni.upx2px(420);
			this.getServerData();
		},
		computed: {},
		mounted() {
			// this.shishi();
		},
		methods: {
			startTest() {
				let opts = {
					url: 'dz/startHumiture',
					method: 'post'
				};
				request.httpRequest(opts).then(res => {
					console.log("!!!!!!!!!!");
					console.log(res);
					if (res.statusCode == 200) {
						console.log("开始测试");
						layer.msg("开始测试");
					} else {}
				});
			},
			stopTest() {
				let opts = {
					url: 'dz/cancleHumiture',
					method: 'post'
				};
				request.httpRequest(opts).then(res => {
					console.log(res);
					if (res.statusCode == 200) {
						console.log("结束测试");
						layer.msg("停止测试");
					} else {}
				});
			},
			shishi() {
				_self.timer = setInterval(() => {
					this.getData();
				}, 3000);
			},
			getData() {
				console.log('数据加载');
				let opts = {
					url: 'dz/getHumiture',
					method: 'get'
				};
				uni.showLoading({
					title: '加载中'
				});
				request.httpRequest(opts).then(res => {
					console.log(res);
					uni.hideLoading();
					if (res.statusCode == 200) {
						_self.humidity.series[0].data = res.data.humidity;
						_self.updateCharts(_self.humidity);
					} else {}
				});
			},
			getServerData() {
				_self.showGauge('canvasGauge', this.Gauge);
				_self.showHumidity('canvasHumidity', this.humidity);
			},
			// 仪表盘
			showGauge(canvasId, chartData) {
				const ctx = uni.createCanvasContext(canvasId, this);
				canvaGauge = new uCharts({
					$this: _self,
					canvasId: canvasId,
					context: ctx,
					type: 'gauge',
					fontSize: 11,
					legend: false,
					title: {
						name: Math.round(_self.chartData.series[0].data * 100) + '%',
						color: _self.chartData.categories[1].color,
						fontSize: 25 * _self.pixelRatio,
						offsetY: 50 * _self.pixelRatio //新增参数，自定义调整Y轴文案距离
					},
					subtitle: {
						name: _self.chartData.series[0].name,
						color: '#666666',
						fontSize: 15 * _self.pixelRatio,
						offsetY: -50 * _self.pixelRatio //新增参数，自定义调整Y轴文案距离
					},
					extra: {
						gauge: {
							type: 'default',
							width: _self.gaugeWidth * _self.pixelRatio, //仪表盘背景的宽度
							startAngle: 0.75,
							endAngle: 0.25,
							startNumber: 0,
							endNumber: 100,
							splitLine: {
								fixRadius: 0,
								splitNumber: 10,
								width: _self.gaugeWidth * _self.pixelRatio, //仪表盘背景的宽度
								color: '#FFFFFF',
								childNumber: 5,
								childWidth: _self.gaugeWidth * 0.4 * _self.pixelRatio //仪表盘背景的宽度
							},
							pointer: {
								width: _self.gaugeWidth * 0.8 * _self.pixelRatio, //指针宽度
								color: 'auto'
							}
						}
					},
					background: '#FFFFFF',
					pixelRatio: _self.pixelRatio,
					categories: _self.chartData.categories,
					series: _self.chartData.series,
					animation: true,
					width: _self.cWidth * _self.pixelRatio,
					height: _self.cHeight * _self.pixelRatio,
					dataLabel: true
				});
			},
			showHumidity(canvasId, humidity) {
				const ctx1 = uni.createCanvasContext(canvasId, this);
				canvasHumidity = new uCharts({
					$this: _self,
					canvasId: canvasId,
					context: ctx1,
					type: 'gauge',
					color: ['#1890FF', '#91CB74', '#FAC858', '#EE6666', '#73C0DE', '#3CA272', '#FC8452', '#9A60B4',
						'#ea7ccc'
					],
					padding: undefined,
					title: {
						name: _self.humidity.series[0].data,
						fontSize: 25,
						color: '#2fc25b',
						offsetY: 0
					},
					subtitle: {
						name: '湿度',
						fontSize: 15,
						color: '#1890ff',
						offsetY: 0
					},
					extra: {
						gauge: {
							type: 'progress',
							width: 20,
							labelColor: '#666666',
							startAngle: 0.75,
							endAngle: 0.25,
							startNumber: 0,
							endNumber: 100,
							labelFormat: '',
							splitLine: {
								fixRadius: -10,
								splitNumber: 10,
								width: 15,
								color: '#FFFFFF',
								childNumber: 5,
								childWidth: 12
							},
							pointer: {
								width: 24,
								color: 'auto'
							}
						}
					},
					background: '#FFFFFF',
					pixelRatio: _self.pixelRatio,
					categories: _self.humidity.categories,
					series: _self.humidity.series,
					animation: true,
					width: _self.cWidth * _self.pixelRatio,
					height: _self.cHeight * _self.pixelRatio,
					dataLabel: true
				});
			},
			// ucharts end
			updateCharts(humidity) {
				canvasHumidity.updateData({
					categories: humidity.categories,
					series: humidity.series,
					animation: false
				});
				console.log(_self.humidity.series[0].data);
			}
		},
		beforeDestroy() {
			if (_self.timer) {
				//如果定时器还在运行 或者直接关闭，不用判断
				clearInterval(_self.timer); //关闭
			}
		}
	};
</script>

<style lang="scss" scoped>
	.chartsMain {
		width: 100%;
		height: 450rpx;
		padding-top: 15rpx;
		background: #fff;
		margin-bottom: 24rpx;
		border-top: 2rpx solid #f2f2f2;

		.charts {
			width: 100%;
			height: 450rpx;
			box-sizing: border-box;
		}
	}

	.Starttest {
		// margin-left: 40px;
	}

	.Stoptest {
		// margin-left: 50px;
		// margin-right: 20%;
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

		.btns {			margin-bottom: 20px;			display: flex;			width: 100%;			align-items: center;			justify-content: center;		}
	}
</style>
