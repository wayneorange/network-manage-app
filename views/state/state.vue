<template>
	<view class="state-container">
		<u-sticky offset-top="" :enable="enable">
			<view class="sate-header">
				<div class="state-equipment-title">
					<span @click="selectEquipment">{{ selectEqItem.label }}</span>
					<u-icon style="margin: 0 5px;" name="arrow-down"></u-icon>
					<u-select v-model="equipmentShow" :list="equipmentList" :default-value="[selectEqIndex]"
						@confirm="confirmSelect"></u-select>
				</div>
				<div class="state-status">
					<span>正常上网</span>
				</div>
				<div class="state-more-box">
					<div class="left-box">
						<div class="item-content" v-for="item in 2">
							<span class="num">2.6</span>
							<div class="item-box">
								<span>上行</span>
								<span>MB/S</span>
							</div>
						</div>
					</div>
					<div class="more" dir="rtl">
						<u-icon name="arrow-right"></u-icon>
						<span>上网设置</span>
					</div>
				</div>
			</view>
			<view class="line-content">
				<div style="font-weight: 700;font-size: 1.25em;">主人网络</div>
				<div style="color:  #6e6e6e" @click="masterSetting">
					<span>TP-Link-Master</span>
					<u-icon name="setting"></u-icon>
				</div>
			</view>
		</u-sticky>
		<view class="state-main">
			<div class="list-content">
				<!-- list -->
				<view class="list-item" v-for="(item, i) in netList" :key="i">
					<img :src="item.img" class="img" alt="" srcset="">
					<view class="content">
						<text class="name" :class="item.isBj ? 'isBj':''">{{ item.title }}</text>
						<view class="line">
							<view class="up">
								<u-icon name="arrow-upward" color="#88b3de"></u-icon>
								<text class="num">{{ item.up }}</text>
							</view>
							<view class="down">
								<u-icon name="arrow-downward" color="#bfe3d7"></u-icon>
								<text class="num">{{ item.down }}</text>
							</view>
						</view>
						<view class="line">
							<text class="jr">接入{{ item.in }}</text>
							<text
								style="display: flex;align-items: center;">{{ item.time || new Date().Format("yyyy-MM-dd mm:ss") }}</text>
						</view>

					</view>
				</view>
				<!--  -->
			</div>
			<div class="more-list">
				<span style="color: #8b8b8b;">展开<u-icon style="margin: 0 5px;" name="arrow-down"></u-icon></span>
				<span style="color: #7c99b5;font-weight: 500;">设备总表</span>
			</div>
		</view>
		<view class="visitor-config">
			<text>访客网络</text>
			<text class="to-open" @click="openVisitor">去开启</text>
		</view>

		<!--  -->
		<u-toast ref="uToast" />
	</view>
</template>

<script>
	export default {
		data() {
			return {
				equipmentShow: false,
				equipmentList: [{
						value: 'lyq',
						label: '路由器'
					},
					{
						value: 'rd',
						label: '热点'
					}
				],
				selectEqItem: {
					value: 'lyq',
					label: '路由器'
				},
				netList: [{
					title: '音响',
					img: 'static/state/yinxiang.png',
					up: '2.8KB/S',
					down: '12.8MB/S',
					in: '2.4G',
					isBj: true,
					time: ''
				}, {
					title: 'DVD',
					img: 'static/state/DVD.png',
					up: '2.8KB/S',
					down: '12.8MB/S',
					in: '2.4G',
					isBj: false,
					time: ''
				}, {
					title: '未知设备',
					img: 'static/state/equip.png',
					up: '2.8KB/S',
					down: '12.8MB/S',
					in: '2.4G',
					isBj: false,
					time: ''
				}, {
					title: '未知设备-0',
					img: 'static/state/equip.png',
					up: '2.8KB/S',
					down: '12.8MB/S',
					in: '2.4G',
					isBj: false,
					time: ''
				}, ],
				enable: true
			};
		},
		// 在对应的show和hide页面生命周期中打开或关闭监听
		onShow() {
			this.enable = true
		},
		onHide() {
			this.enable = false
		},
		computed: {
			selectEqIndex() {
				const index = this.equipmentList.findIndex(o => {
					return o.value === this.selectEqItem.value
				});
				return index >= 0 ? index : 0;
			}
		},
		methods: {
			selectEquipment() {
				this.equipmentShow = !this.equipmentShow;
			},
			confirmSelect(e) {
				this.selectEqItem = e[0];
			},
			masterSetting() {
				this.$refs.uToast.show({
					title: '待开发',
					position: 'top',
					type: 'warning'
				})
			},
			openVisitor() {
				this.$refs.uToast.show({
					title: '待开发',
					position: 'top',
					type: 'warning'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.state-container {
		height: calc(100vh - 50px);
		width: 100%;
		margin-bottom: 50px;

		.sate-header {
			height: 25%;
			width: 100%;
			background-image: linear-gradient(to bottom right, #4f60e4, #5d9ff6);
			color: #ffffff;

			.state-equipment-title {
				width: 100%;
				padding: 30px 0;
				display: flex;
				align-items: center;
				justify-content: center;
			}

			.state-status {
				display: inline-block;
				padding: 5px 10px;
				margin: 10px 20px;
				border: 1px solid #FFFFFF;
				border-radius: 5px;
			}

			.state-more-box {
				height: calc(100% - 90px - 3em);
				width: 100%;
				display: flex;
				align-items: center;

				.left-box {
					height: 100%;
					width: 70%;
					display: flex;
					align-items: center;

					.item-content {
						display: flex;
						align-items: center;
						margin: 20px;

						.num {
							font-size: 2.5em;
							margin-right: 5px;
						}

						.item-box {
							display: flex;
							flex-direction: column;
							font-size: 0.75em;
							color: #dfe1e5;
						}
					}
				}

				.more {
					width: 30%;
					float: right;
					padding-right: 10px;
				}
			}
		}

		.line-content {
			height: 60px;
			background-color: #fff;
			display: flex;
			align-items: center;
			padding: 20px;
			justify-content: space-between;

			.u-icon {
				margin-left: 10px;
			}
		}

		.state-main {
			width: 100%;

			.list-content {

				.list-item {
					display: flex;
					align-items: center;
					height: 100px;
					padding: 20px;
					background-color: #fff;
					border-bottom: 1px solid #f2f2f2;

					.img {
						height: 80px;
						width: 80px;
						margin-right: 20px;
					}

					.content {

						.name {
							font-size: 1.25em;
							font-weight: 500;
						}

						.name.isBj {
							font-size: 1.25em;
							font-weight: 500;

							&::after {
								content: '本机';
								margin-left: 10px;
								font-size: .7em;
								display: inline-flex;
								align-items: center;
								justify-content: center;
								background-color: #3a74ce;
								border-radius: 5px;
								color: #fff;
								padding: 2px 5px;
							}
						}

						.line {
							display: flex;
							white-space: nowrap;
							margin-top: 5px;
							color: #838383;

							.up,
							.down,
							.jr {
								margin-right: 20px;

								.u-icon {
									margin-right: 5px;
								}
							}

						}
					}
				}

			}

			.more-list {
				width: 100%;
				box-sizing: border-box;
				background-color: #fff;
				display: flex;
				align-items: center;
				justify-content: space-between;
				height: 3em;
				padding: 0 20px;
			}
		}

		.visitor-config {
			width: 100%;
			padding: 20px;
			margin-top: 10px;
			background-color: #fff;
			display: flex;
			align-items: center;
			justify-content: space-between;

			.to-open {
				display: inline-flex;
				align-items: center;
				justify-content: center;
				background-color: #f4f5f5;
				height: 2.5em;
				border-radius: 1.25em;
				width: 100px;
				color: #6995c7;
				font-weight: 500;

			}
		}
	}
</style>
