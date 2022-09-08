<template>
	<view>
		<cu-custom bgColor="bg-gradual-blue" :isBack="true">
			<block slot="backText">返回</block>
			<block slot="content">网速测试</block>
		</cu-custom>
		<u-notice-bar mode="horizontal" type="primary" :list="list"></u-notice-bar>
		<!-- 按钮 -->
		<view>
			<button size="mini" class="Starttest" plain="true">开始测试</button>
			<button size="mini" class="Stoptest" plain="true">停止测试</button>
		</view>
		<!-- 仪表盘 -->
		<view class="cu-bar bg-white margin-top-xs">
			<view class="gauge"><text class="text-xl text-bold text-blue text-shadow">网速测试</text></view>
		</view>

		<view class="guage-charts-box"><qiun-data-charts type="gauge" :opts="opts" :chartData="chartData" /></view>

		<!-- 折线图 -->
		<view class="cu-bar bg-white margin-top-xs">
			<view class="gauge"><text class="text-xl text-bold text-blue text-shadow">历史速度数据</text></view>
		</view>

		<view class="line-charts-box"><qiun-data-charts type="line" :opts="LineOpts" :chartData="LineChartData" /></view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			list: ['当前页面为网速测试'],
			LineChartData: {},
			LineOpts: {},
			chartData: {},
			//仪表盘的基本配置
			//您可以通过修改 config-ucharts.js 文件中下标为 ['gauge'] 的节点来配置全局默认参数，如都是默认参数，此处可以不传 opts 。实际应用过程中 opts 只需传入与全局默认参数中不一致的【某一个属性】即可实现同类型的图表显示不同的样式，达到页面简洁的需求。
			opts: {
				color: ['#1890FF', '#91CB74', '#FAC858', '#EE6666', '#73C0DE', '#3CA272', '#FC8452', '#9A60B4', '#ea7ccc'],
				padding: undefined,
				title: {
					name: '100Mbps/s',
					fontSize: 22,
					color: '#2fc25b',
					offsetY: 0
				},
				subtitle: {
					name: '实时速度',
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
				}
			},
			LineOpts: {
				color: ['#1890FF', '#91CB74', '#FAC858', '#EE6666', '#73C0DE', '#3CA272', '#FC8452', '#9A60B4', '#ea7ccc'],
				padding: [15, 10, 0, 15],
				legend: {},
				xAxis: {
					disableGrid: true
				},
				yAxis: {
					gridType: 'dash',
					dashLength: 2,
					data: [
						{
							min: 0,
							max: 100
						}
					]
				},
				extra: {
					line: {
						type: 'straight',
						width: 2
					}
				}
			}
		};
	},
	onReady() {
		this.getServerData();
	},
	methods: {
		getServerData() {
			//模拟从服务器获取数据时的延时
			setTimeout(() => {
				//模拟服务器返回数据，如果数据格式和标准格式不同，需自行按下面的格式拼接
				let res = {
					categories: [{ value: 0.2, color: '#1890ff' }, { value: 0.8, color: '#2fc25b' }, { value: 1, color: '#f04864' }],
					series: [
						{
							name: '实时速度',
							data: 1
						}
					]
				};
				this.chartData = JSON.parse(JSON.stringify(res));
				//折线图的ChartsData
				let Lineres = {
					categories: ['2016', '2017', '2018', '2019', '2020', '2021'],
					series: [
						{
							name: '历史网速',
							data: [35, 36, 31, 33, 10, 34]
						}
					]
				};
				this.LineChartData = JSON.parse(JSON.stringify(Lineres));
			}, 500);
		}
	}
};
</script>

<style lang="scss" scoped>
.Starttest {
	margin-left: 40px;
}
.Stoptest {
	margin-left: 50px;
	margin-right: 20%;
}
.gauge {
	margin-left: 20px;
}
/* 请根据实际需求修改父元素尺寸，组件自动识别宽高 */
.guage-charts-box {
	width: 100%;
	height: 280px;
	margin-top: 30px;
}
.line-charts-box {
	width: 100%;
	height: 300px;
}
</style>
