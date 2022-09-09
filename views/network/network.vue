<template>
	<view class="network-container">
		<canvas id="canvas" canvas-id="canvas" width="100%" height="100%"></canvas>
		<img src="static/network/luyou.png" class="img">
	</view>
</template>

<script>
	import jtopo from 'jtopo2';
	export default {
		data() {
			return {
				nodes: [
					{
						title: ''
					}
				]
			};
		},
		mounted() {
			this.drawCanvas();
		},
		methods: {
			drawCanvas() {
				// 桌面信息
				const screenInfo = uni.getSystemInfoSync();
				// uniapp选择元素节点
				const query = uni.createSelectorQuery().in(this);
				const width = screenInfo.windowWidth;
				const ctx = uni.createCanvasContext('canvas');
				console.log(screenInfo);
				// 
				ctx.drawImage('../../static/network/network.png', (width / 2) - 40, 40, 80, 80);
				ctx.drawImage('../../static/network/luyou.png', (width / 2) - 50, 200, 100, 100);
				ctx.moveTo((width / 2), 200);
				ctx.lineTo(300,120);
				ctx.lineTo(300,600);
				ctx.lineWidth=10;
				ctx.strokeStyle="blue";
				ctx.stroke();
				ctx.draw()
			
			}
		}
	}
</script>

<style lang="scss">
.network-container {
	width: 100%;
	height: 100%;
	border: 1px solid red;
	background: #FFFFFF;
	
	uni-canvas {
		height: 100%;
		width: 100%;
	}
}
</style>
