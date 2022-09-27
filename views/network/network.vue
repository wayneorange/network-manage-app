<template>
	<view class="network-container">
		<canvas id="canvas" canvas-id="canvas" width="100%" height="100%"></canvas>
		<text style="" class="luyou">路由器</text>
		<view class="text-contetnt">
			<text :style="{height: `${110 + ((Math.ceil((i + 1) / 4) - 1)) * 5}px`}" v-for="(item, i) in imgs" :key="i + item.text">{{ item.text }}</text>
		</view>
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
				],
				imgs: [
					{
						text: 'My Phone',
						img: '../../static/network/apple.png'
					},
					{
						text: 'Dell',
						img: '../../static/network/dell.png'
					},
					{
						text: 'Dell',
						img: '../../static/network/dell.png'
					},
					{
						text: 'Dell',
						img: '../../static/network/dell.png'
					},
					{
						text: 'Dell',
						img: '../../static/network/dell.png'
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
				const screenWidth = screenInfo.windowWidth;
				const color = '#4875b3';
				const ctx = uni.createCanvasContext('canvas');
				// 
				const centerScreen = Number(screenWidth / 2);
				ctx.drawImage('../../static/network/network.png', centerScreen - 40, 40, 80, 80);
				ctx.drawImage('../../static/network/luyou.png', centerScreen - 40, 180, 80, 80);
				ctx.moveTo(centerScreen, 180);
				ctx.lineTo(centerScreen, 150);
				ctx.lineWidth = 2.5;
				ctx.strokeStyle= color;
				ctx.stroke();
				
				ctx.beginPath()
				ctx.arc(centerScreen, 150, 3, 0, 2 * Math.PI)
				ctx.setFillStyle(color);
				ctx.stroke();
				ctx.fill();
				ctx.closePath()
				
				const imgs = this.imgs || [];
				const num = ((screenWidth / 4) - 80) / 2;
				imgs.forEach((o, i) => {
					if(o) {
						ctx.drawImage(o.img, ((screenWidth / 4) * (i % 4)) + num, (Math.ceil((i + 1) / 4) - 1) * 120 + 350 , 80, 80);
					}
				})
				
				const startLine = (screenWidth / 4) / 2;
				const endLine = screenWidth - startLine;
				ctx.beginPath();
				ctx.moveTo(startLine, 350);
				ctx.lineTo(startLine, 345);
								
				// 画弧线
				ctx.arcTo(startLine, 330, startLine + 5, 330, 5); // 创建弧
				
				ctx.moveTo(startLine + 5, 330);
				ctx.lineTo(centerScreen, 330);
				ctx.lineTo(centerScreen, 310);
				ctx.lineTo(centerScreen, 330);
				ctx.lineTo(endLine - 5, 330);
				
				// 画弧线
				ctx.arcTo(endLine, 330, endLine, 335, 5); // 创建弧

				ctx.lineTo(endLine, 350);
				ctx.lineWidth = 2.5;
				ctx.strokeStyle= '#cecece';
				ctx.stroke();
				ctx.closePath();
				
				ctx.beginPath();
				ctx.arc(centerScreen, 310, 4, 0, 2 * Math.PI)
				ctx.setFillStyle('#cecece');
				ctx.closePath();
				ctx.fill();
				
				ctx.draw()
			
			}
		}
	}
</script>

<style lang="scss">
.network-container {
	width: 100%;
	height: 100%;
	background: #FFFFFF;
	position: relative;
	
	.luyou {
		left: 50%;
		transform: translateX(-50%);
		top: 265px;
		position: absolute;
	}
	
	.text-contetnt {
		position: absolute;
		top: 350px;
		width: 100%;
		display: grid;
		grid-template-columns: 25% 25% 25% 25%;
		overflow: hidden;
		
		text {
			height: 110px;
			display: flex;
			align-items: flex-end;
			justify-content: center;
		}
	}
	
	text {
		font-size: 1.25em;
		font-weight: 500;
	}
	
	uni-canvas {
		height: 100%;
		width: 100%;
	}
	
}
</style>
