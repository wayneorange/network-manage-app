<template>
	<view class="equipManage-container">
		<view class="swiper-header">
			<h1 class="title">设备管理</h1>
			<u-tabs-swiper ref="uTabs" :list="list" :current="current" @change="tabsChange" :is-scroll="false"
				swiperWidth="750"></u-tabs-swiper>
		</view>

		<view class="swiper-container">
			<view class="swiper-tip">
				<text>有6台上网设备</text>
				<view class="right">
					<text @click="selectMap">{{ selectEqItem.label }}</text>
					<u-icon name="arrow-down" size="28" @click="selectMap"></u-icon>
					<u-select v-model="equipmentShow" :list="equipmentList" :default-value="[selectEqIndex]"
						@confirm="confirmSelect"></u-select>
				</view>
			</view>
			<swiper :current="swiperCurrent" @transition="transition" @animationfinish="animationfinish">
				<swiper-item class="swiper-item" v-for="(item, index) in list" :key="index">
					<scroll-view scroll-y style="height: 700px; width: 100%;" @scrolltolower="onreachBottom">
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
					</scroll-view>
				</swiper-item>
			</swiper>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [{
					name: '主人网络'
				}, {
					name: '访客网络'
				}, {
					name: '已禁设备'
				}],
				// 因为内部的滑动机制限制，请将tabs组件和swiper组件的current用不同变量赋值
				current: 0, // tabs组件的current值，表示当前活动的tab选项
				swiperCurrent: 0, // swiper组件的current值，表示当前那个swiper-item是活动的
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
				equipmentShow: false,
				equipmentList: [{
						value: 'all',
						label: '全部'
					},
					{
						value: 'one',
						label: '分类一'
					}
				],
				selectEqItem: {
					vaue: 'all',
					label: '全部'
				}
			};
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
			// tabs通知swiper切换
			tabsChange(index) {
				this.swiperCurrent = index;
			},
			// swiper-item左右移动，通知tabs的滑块跟随移动
			transition(e) {
				let dx = e.detail.dx;
				this.$refs.uTabs.setDx(dx);
			},
			// 由于swiper的内部机制问题，快速切换swiper不会触发dx的连续变化，需要在结束时重置状态
			// swiper滑动结束，分别设置tabs和swiper的状态
			animationfinish(e) {
				let current = e.detail.current;
				this.$refs.uTabs.setFinishCurrent(current);
				this.swiperCurrent = current;
				this.current = current;
			},
			// scroll-view到底部加载更多
			onreachBottom() {

			},
			selectMap() {
				this.equipmentShow = true
			},
			confirmSelect(e) {
				this.selectEqItem = e[0];
			}
		}
	}
</script>

<style lang="less">
	uni-page-body {
		overflow: hidden;
	}
	.equipManage-container {
		width: 100%;
		height: 100%;
		background-color: #fff;

		.swiper-header {
			width: 100%;
			padding: 20px 20px 0 20px;
		}

		.title {
			margin-bottom: 20px;
		}

		.swiper-container {
			background-color: #f2f2f2;
			padding: 10px 20px;
			width: 100%;
			height: calc(100% - 100px);

			.swiper-tip {
				display: flex;
				align-items: center;
				justify-content: space-between;
				margin: 10px 0;
				color: #bfbfbf;

				.right text {
					margin-right: 5px;
				}
			}

			uni-swiper {
				height: 100%;
			}

			.list-content {
				height: 100%;
				width: 100%;

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
		}
	}
</style>
