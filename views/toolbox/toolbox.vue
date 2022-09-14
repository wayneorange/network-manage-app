<template>
	<view class="tooltip-contaier">
		<view class="tooltip-list" v-for="(item, i) in list" :key="item.title + i">
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
						img: '../../static/network/dell.png',
						url: ''
					},
					{
						title: '温湿度图',
						img: '../../static/network/dell.png',
						url: ''
					},
					{
						title: '路由器测试',
						img: '../../static/network/dell.png',
						url: ''
					},
					{
						title: '图表历史记录',
						img: '../../static/network/dell.png',
						url: ''
					},
					{
						title: '实时环境检测历史记录',
						img: '../../static/network/dell.png',
						url: ''
					},
					{
						title: '实时姿态检测历史记录',
						img: '../../static/network/dell.png',
						url: ''
					}
				]
			};
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
