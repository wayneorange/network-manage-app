<template>
	<view class="lineBroken">
		<view class="flowPassenger">
			<em></em><text>进店客流</text><text class="seeDetail" @click="intShopRanking">查看详情</text>
			<view style="width: 46rpx;">
				<!-- <image src="../static/img/triangleArrow.svg" mode="aspectFit"></image> -->
			</view>
		</view>
		
		<view class="lineChart">
			<view class="histogram_chart">
				<canvas :canvasId="canvasId" id="canvasId" disable-scroll="true"
					:style="{ width: cWidth + 'px', height: cHeight + 'px' }" @touchstart="touchstart"
					@touchmove="touchmove" @touchend="touchend" />
				<slot />
			</view>
		</view>
	</view>

</template>

<script>
	import uCharts from "@/components/u-charts/u-charts.js"; //可以优化放全局 uCharts ==>使用全局
	const lineCharts = {},
		optionAs = {};
	export default {
		name: 'LineChart',
		props: {
			dataAs: {
				//数据
				type: Object,
				default: () => ({})
			},
			basicAs: {
				////通用基础项设置
				type: Object,
				default: () => ({})
			},
			xAxisAs: {
				//xAxis YAxis等轴线基础设置(圆环饼图无轴线无需设置)
				type: Object,
				default: () => ({})
			},
			yAxisAs: {
				//xAxis YAxis等轴线基础设置(圆环饼图无轴线无需设置)
				type: Object,
				default: () => ({})
			},
			legendAs: {
				//图例设置
				type: Object,
				default: () => ({})
			},
			extraAs: {
				//详情请看 http://doc.ucharts.cn/1172130k
				type: Object,
				default: () => ({})
			},
			width: {
				//图标宽度
				type: Number,
				default: 750
			},
			height: {
				//图标高度
				type: Number,
				default: 500
			},
			labelKey: {
				type: String,
				default: 'categories'
			},
			valueKey: {
				type: String,
				default: 'series'
			},
			canvasId: {
				type: String,
				default: `line_canvas_${Math.ceil(Math.random(5) * 10000)}`
			}
		},
		data() {
			return {};
		},
		computed: {
			cWidth() {
				return uni.upx2px(this.width);
			},
			cHeight() {
				return uni.upx2px(this.height);
			}
		},
		mounted() {
			this.initData();
		},
		watch: {
			dataAs() {
				this.dataAs = this.dataAs
				console.log(this.dataAs)
				this.initData();
			},
		},
		methods: {
			/*父组件调用*/
			lineStatistical: function() {
				console.log(this.dataAs)
				// this.initData();
			},
			initData: function() {
				let defaultOption = {
					//通用基础项设置 basicAs
					$this: this, //this实例组件内使用图表，必须传入this实例
					canvasId: this.canvasId, //页面组件canvas-id，支付宝中为id
					type: 'line', //图表类型，可选值为pie、line、column、area、ring、radar、arcbar、gauge、candle、bar、mix、rose、word
					animation: true, //是否动画展示
					dataLabel: false, //是否在图表中显示数据标签内容值
					dataPointShape: true,
					duration: 1000, //动画展示时长单位毫秒
					fontSize: 12, //全局默认字体大小（可选，单位为px，默认13px）高分屏不必乘像素比，自动根据pixelRatio计算
					background: '#ffffff', //canvas背景颜色（如果页面背景颜色不是白色请设置为页面的背景颜色，默认#ffffff）无作用
					pixelRatio: 1, //像素比，默认为1，仅支付宝小程序需要大于1，其他平台必须为1
					width: this.cWidth, //canvas宽度，单位为px，支付宝高分屏需要乘像素比(pixelRatio)
					height: this.cHeight, //canvas高度，单位为px，支付宝高分屏需要乘像素比
					//数据列表配置项 dataAS
					categories: this.dataAs[this.labelKey], //数据类别(饼图、圆环图不需要)
					series: this.dataAs[this.valueKey], //数据列表
					//坐标轴配置项 axisAs
					xAxis: {
						type: 'grid',
						gridColor: '#CCCCCC',
						gridType: 'dash',
						dashLength: 8,
						rotateLabel: 'true',
						fontSize: 2,
						fontColor: '#FFFFFF',
					},
					yAxis: {
						//如果是多坐标系则传数组型对象[{disabled:true},{disabled:false}]
						disabled: false, //不绘制Y轴
						position: 'left', //Y轴位置，可选值左侧left右侧right(未起效果)
						format: val => {
							let defaultSetting = {
								type: 'number',
								fixed: 0,
								name: '人'
							};
							let {
								type,
								fixed,
								name
							} = this.yAxisAs && this.yAxisAs.formatter ? Object.assign(defaultSetting, this
								.yAxisAs.formatter) : defaultSetting;
							if (type == 'number') {
								return val.toFixed(fixed) + name;
							} else if (type == 'percent') {
								let newName = name || '%';
								return (val * 100).toFixed(fixed) + newName;
							} else {
								return val.toFixed(0) + '人';
							}
						}
						// title:'Y轴标题',//Y轴标题
						// titleFontSize:basic.fontSize titleFontColor==>#666666 fontColor==>666666 Y轴数据点颜色 fontSizeY轴数据标签字体大小 等详见http://doc.ucharts.cn/1172128
					},
					//图列配置 legendAs
					legend: {
						show: true, //是否显示各类别的图例标识
					},
					//扩展配置 extraAs 详情请看 http://doc.ucharts.cn/1172130
					extra: {
						line: {
							type: 'straight'
						}
					}
				};
				optionAs[this.canvasId] = Object.assign(defaultOption, this.basicAs, this.xAxisAS, this.yAxisAS, this
					.legendAs, this.extraAs);
				lineCharts[this.canvasId] = new uCharts(optionAs[this.canvasId]);
			},
			touchstart(e) {
				let that = this;
				lineCharts[this.canvasId].touchLegend(e, {
					animation: false
				});
				lineCharts[this.canvasId].scrollStart(e);
				lineCharts[this.canvasId].showToolTip(e, {
					//修改点击事弹出文字
					format: function(item, category) {
						let defaultSetting = {
							type: 'number',
							fixed: 0,
							name: ''
						};
						let newName;
						let {
							type,
							fixed,
							name
						} = that.yAxisAs && that.yAxisAs.formatter ? Object.assign(defaultSetting, that.yAxisAs
							.formatter) : defaultSetting;
						if (typeof item.data === 'object') {
							if (type == 'number') {
								return `${category} ${item.name}:${item.data.value.toFixed(fixed)}${name}`;
							} else if (type == 'percent') {
								newName = name || '%';
								return `${category} ${item.name}:${(item.data.value * 100).toFixed(fixed)}${newName}`;
							} else {
								return `${category} ${item.name}:${item.data.value}`;
							}
						} else {
							if (type == 'number') {
								return `${category} ${item.name}:${item.data.toFixed(fixed)}${name}`;
							} else if (type == 'percent') {
								newName = name || '%';
								return `${category} ${item.name}:${(item.data * 100).toFixed(fixed)}${newName}`;
							} else {
								return `${category} ${item.name}:${item.data}`;
							}
						}
					}
				});
			},
			touchmove(e) {
				lineCharts[this.canvasId].scroll(e);
			},
			touchend(e) {
				lineCharts[this.canvasId].scrollEnd(e);
			},
			intShop() {
				uni.navigateTo({
					url: '/pages/intShopDetail/intShopDetail'
				});
			},
		}
	};
</script>


<style>
	.lineBroken {
		width: 100%;
		margin-top: 98rpx;
		display: flex;
		flex-wrap: wrap;
		margin-bottom: 182rpx;
	}

	.flowPassenger {
		width: 100%;
		height: 42rpx;
		display: flex;
		font-size: 32rpx;
	}

	.flowPassenger em {
		width: 12rpx;
		height: 34rpx;
		/* border: 1rpx solid rgb(0,82,254); */
		background: rgb(0, 82, 254);
		border-radius: 6rpx;
		margin: 4rpx 0 0 30rpx;
	}

	.flowPassenger text {
		width: 36%;
		height: 42rpx;
		color: rgb(0, 82, 254);
		margin: 0 0 0 20rpx;
	}

	.flowPassenger>.seeDetail {
		width: 18%;
		height: 42rpx;
		color: rgb(0, 82, 254);
		margin-left: 28%;
	}

	.flowPassenger image {
		width: 100%;
		height: 46rpx;
		margin-top: 3rpx;
	}

	.lineChart {
		margin-top: 70rpx;
		/* width: 750rpx; */
		height: 500rpx;
		display: flex;
		background-color: #FFFFFF;
	}
</style>
