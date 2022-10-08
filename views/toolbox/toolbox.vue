<template>
	<view class="tooltip-contaier">
		<cu-custom bgColor="bg-gradual-blue">
			<block class="text-lg" slot="content">工具箱</block>
		</cu-custom>
		<view class="tooltip-list" v-for="(item, i) in list" :key="item.title + i" @click="showDetail(item)">
			<view class="left">
				<img :src="item.img">
				<text class="name">{{ item.title }}</text>
			</view>
			<view class="right">
				<u-icon name="arrow-right"></u-icon>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [
					{
						title: '九轴测试图',
						img: '../../static/tooltip/number-axis.png',
						url: '/pages/router/nineShaft'
					},
					{
						title: '温湿度图',
						img: '../../static/tooltip/sdwd.png',
						url: '/pages/router/tempetura'
					},
					{
						title: '路由器测试',
						img: '../../static/tooltip/luyou.png',
						url: '/pages/router/routerDiagram'
					},
					{
						title: '图表历史记录',
						img: '../../static/tooltip/ls.png',
						url: '/pages/router/routerDiagram'
					},
					{
						title: '实时环境检测历史记录',
						img: '../../static/tooltip/hjjc.png',
						url: '/pages/router/humidityHis'
					},
					{
						title: '实时姿态检测历史记录',
						img: '../../static/tooltip/sn106.png',
						url: '/pages/router/nineShaftHis'
					},
					{
						title: '信号强度历史记录',
						img: '../../static/tooltip/xh.png',
						url: '/pages/router/signalHis'
					}
				]
			};
		},
		methods: {
			showDetail: function(item) {
				uni.navigateTo({
					url: item.url,
					success(res) {
					},
					fail(err) {
					}
				});
			}
		}
	}
</script>

<style lang="scss">
.tooltip-contaier {
	height: 100%;
	width: 100%;
	background-color: #fff;
	overflow-y: scroll;
	
	.tooltip-list {
		height: 80px;
		width: 100%;
		border-bottom: 1px solid #dadce0;
		display: flex;
		align-items: center;
		justify-content: space-between;
		
		.left {
			height: 100%;
			display: flex;
			align-items: center;
			margin-left: 20px;
			
			img {
				height: 60px;
				width: 60px;
				border-radius: 10px;
				margin-right: 20px;
			}
			
			.name {
				font-size: 1.25em;
				font-weight: 600;
			}
		}
		
		.right {
			height: 100%;
			display: flex;
			align-items: center;
			margin-right: 20px;
		}
	}
}
</style>
