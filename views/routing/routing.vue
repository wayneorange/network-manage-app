<template>
	<view class="routting-container">
		<view class="header-container">
			<view class="email" dir="rtl">
				<u-icon name="email" color="#4682d9" size="60"></u-icon>
			</view>
			<text class="title">路由设置</text>
		</view>
		<view class="list-container">
			<view class="list-item" v-for="(item, i) in 10" :key="i" :style="{ marginBottom: `${i === 0 || i % 4 === 0 ? '20px': '0'}` }">
				<view class="left">
					<img src="../../static/network/dell.png">
					<text class="name">title</text>
				</view>
				<view class="right">
					<u-icon name="arrow-right"></u-icon>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		}
	}
</script>

<style lang="scss">
.routting-container {
	width: 100%;
	height: 100%;
	background-color: #fff;
	
	.header-container {
		height: 120px;
		width: 100%;
		.email {
			padding: 20px 20px 10px 0;
		}
		
		.title {
			font-size: 3em;
			font-weight: 600;
			padding-left: 20px;
		}
	}
	
	.list-container {
		width: 100%;
		margin-top: 20px;
		height: calc(100% - 140px);
		background-color: #f1f5f9;
		overflow-x: hidden;
		overflow-y: scroll;
		
		.list-item {
			height: 80px;
			width: 100%;
			border-bottom: 1px solid #dadce0;
			display: flex;
			align-items: center;
			justify-content: space-between;
			background-color: #FFFFFF;
			
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
}
</style>
