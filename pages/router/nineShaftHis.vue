<template>
	<view>
		<cu-custom bgColor="bg-gradual-blue" :isBack="true"><block slot="backText">返回</block></cu-custom>
		<u-notice-bar mode="horizontal" type="primary" :list="list"></u-notice-bar>

		<!-- 时间选择器 -->
		<view class="example-body"><uni-datetime-picker type="date" :clear-icon="false" v-model="single" /></view>

		<view class="style">
			<picker @change="bindPickerChange" :value="index" :range="array">
				<view class="uni-input">{{ array[index] }}</view>
			</picker>
		</view>
		<view class="style"><button @click="changeOption">查询</button></view>
		<view @click="echarts.onClick" :prop="option" :change:prop="echarts.updateEcharts" id="echarts" class="echarts"></view>
	</view>
</template>

<script>
import request from '../../common/request.js';
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
			array: ['加速度', '陀螺仪', '角度', '磁场', '温度', '气压和高度'],
			index: 0,

			option: {
				title: {
					text: ''
				},
				tooltip: {},
				legend: {
					data: []
				},
				// grid: {
				// 	left: "3%",
				// 	right: "4%",
				// 	bottom: "3%",
				// 	containLabel: true,
				// },
				// 保存图表照片到本地
				toolbox: {
					feature: {
						saveAsImage: {}
					}
				},
				xAxis: {
					data: []
				},
				yAxis: {},
				series: [
					{
						name: '',
						type: 'line',
						data: []
					},
					{
						name: '',
						type: 'line',
						data: []
					},
					{
						name: '',
						type: 'line',
						data: []
					}
				]
			}
		};
	},
	mounted() {
		setTimeout(() => {
			this.datetimesingle = Date.now() - 2 * 24 * 3600 * 1000;
			this.datetimerange = ['2021-07-08 0:01:10', '2021-08-08 23:59:59'];
		}, 3000);
	},
	onLoad() {},
	methods: {
		change(e) {
			this.single = e;
			console.log('change事件:', e);
		},
		alterOption() {
			var data_legend = [];
			if (this.index == '0') {
				this.option.title.text = '加速度历史记录';
				data_legend[0] = 'acc_x';
				data_legend[1] = 'acc_y';
				data_legend[2] = 'acc_z';
				this.option.legend.data = data_legend;
				this.option.series[0].name = 'acc_x';
				this.option.series[1].name = 'acc_y';
				this.option.series[2].name = 'acc_z';
			} else if (this.index == '1') {
				this.option.title.text = '陀螺仪历史记录';
				data_legend[0] = 'gyro_x';
				data_legend[1] = 'gyro_y';
				data_legend[2] = 'gyro_z';
				this.option.legend.data = data_legend;
				this.option.series[0].name = 'gyro_x';
				this.option.series[1].name = 'gyro_y';
				this.option.series[2].name = 'gyro_z';
			} else if (this.index == '2') {
				this.option.title.text = '角度历史记录';
				data_legend[0] = 'angle_x';
				data_legend[1] = 'angle_y';
				data_legend[2] = 'angle_z';
				this.option.legend.data = data_legend;
				this.option.series[0].name = 'angle_x';
				this.option.series[1].name = 'angle_y';
				this.option.series[2].name = 'angle_z';
			} else if (this.index == '3') {
				this.option.title.text = '磁场历史记录';
				data_legend[0] = 'magnetic_x';
				data_legend[1] = 'magnetic_y';
				data_legend[2] = 'magnetic_z';
				this.option.legend.data = data_legend;
				this.option.series[0].name = 'magnetic_x';
				this.option.series[1].name = 'magnetic_y';
				this.option.series[2].name = 'magnetic_z';
			} else if (this.index == '4') {
				this.option.title.text = '温度历史记录';
				data_legend[0] = 'temperature';
				this.option.legend.data = data_legend;
				this.option.series[0].name = 'temperature';
			} else if (this.index == '5') {
				this.option.title.text = '气压高度历史记录';
				data_legend[0] = 'air_perssure';
				data_legend[1] = 'height';
				this.option.legend.data = data_legend;
				this.option.series[0].name = 'air_perssure';
				this.option.series[1].name = 'height';
			}
		},

		async getHisData() {
			let opts = {
				url: 'dz/getHisNineShaft',
				method: 'get'
			};
			var temp = { choseTime: this.single, tool: this.index };
			this.alterOption();
			console.log(temp);
			await request.httpRequest(opts, JSON.parse(JSON.stringify(temp))).then(res => {
				console.log(res);
				console.log(res.data.length);
				if (res.statusCode == 200) {
					var data_0 = [];
					var data_1 = [];
					var data_2 = [];
					if (this.index == '0') {
						for (let i = 0; i < res.data.length; i++) {
							data_0[i] = res.data[i].acc_x;
							data_1[i] = res.data[i].acc_y;
							data_2[i] = res.data[i].acc_z;
						}
						this.option.series[0].data = data_0;
						this.option.series[1].data = data_1;
						this.option.series[2].data = data_2;
					} else if (this.index == '1') {
						for (let i = 0; i < res.data.length; i++) {
							data_0[i] = res.data[i].gyro_x;
							data_1[i] = res.data[i].gyro_y;
							data_2[i] = res.data[i].gyro_z;
						}
						this.option.series[0].data = data_0;
						this.option.series[1].data = data_1;
						this.option.series[2].data = data_2;
					} else if (this.index == '2') {
						for (let i = 0; i < res.data.length; i++) {
							data_0[i] = res.data[i].angle_x;
							data_1[i] = res.data[i].angle_y;
							data_2[i] = res.data[i].angle_z;
						}
						this.option.series[0].data = data_0;
						this.option.series[1].data = data_1;
						this.option.series[2].data = data_2;
					} else if (this.index == '3') {
						for (let i = 0; i < res.data.length; i++) {
							data_0[i] = res.data[i].magnetic_x;
							data_1[i] = res.data[i].magnetic_y;
							data_2[i] = res.data[i].magnetic_z;
						}
						this.option.series[0].data = data_0;
						this.option.series[1].data = data_1;
						this.option.series[2].data = data_2;
					} else if (this.index == '4') {
						for (let i = 0; i < res.data.legend; i++) {
							data_0[i] = res.data[i].temperature;
						}
						this.option.series[0].data = data_0;
					} else if (this.index == '5') {
						for (let i = 0; i < res.data.length; i++) {
							data_0[i] = res.data[i].air_perssure;
							data_1[i] = res.data[i].height;
						}
						this.option.series[0].data = data_0;
						this.option.series[1].data = data_1;
					}
				} else {
				}
			});
		},

		bindPickerChange: function(e) {
			console.log('picker发送选择改变，携带值为', e.detail.value);
			this.index = e.detail.value;
		},

		changeOption() {
			this.option.series[0].data = [];
			this.option.series[1].data = [];
			this.option.series[2].data = [];
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
			console.log(oldValue);
			console.log(newValue);
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
	width: 40%;
	height: 50px;
	margin-top: 10px;
	display: inline-block;
}

.style {
	/* background-color: #fff; */
	padding: 10px;
	width: 30%;
	height: 30px;
	margin-top: 10px;
	display: inline-block;
}
</style>
