<template>
	<view>
		<index @ShowNews="ShowNews" v-if="PageCur=='index'"></index>
		<search v-if="PageCur=='search'"></search>
		<cases v-if="PageCur=='cases'"></cases>
		<tools v-if="PageCur=='tools'"></tools>
		<me v-if="PageCur=='me'"></me>


		<view class="box">
			<view class="cu-bar tabbar bg-white shadow foot">
				<view class="action" @click="NavChange" data-cur="index">
					<view class='cuIcon-cu-image'>
						<image v-if="PageCur=='index'" src="../../static/tabBar/index_cur.png"></image>
						<image v-if="PageCur != 'index'" src="../../static/tabBar/index.png"></image>
					</view>
					<view :class="PageCur=='index'?'color_main':'text-gray'">首页</view>
				</view>

				<view class="action" @click="NavChange" data-cur="search">
					<view class='cuIcon-cu-image'>
						<image v-if="PageCur=='search'" src="../../static/tabBar/shop_cur.png"></image>
						<image v-if="PageCur != 'search'" src="../../static/tabBar/shop.png"></image>
					</view>
					<view :class="PageCur=='search'?'color_main':'text-gray'">设备列表</view>
				</view>

				<view @click="NavChange" class="action text-gray add-action" data-cur="cases">
					<image class="logo_btn" mode="widthFix" src="../../static/logo.png"></image>
					<view :class="PageCur=='cases'?'color_main':'text-gray'">组件模板</view>
				</view>

				<!-- 工具箱 -->
				<view class="action" @click="NavChange" data-cur="tools">
					<view class='cuIcon-cu-image'>
						<view class="cu-tag badge">
							<!-- 红点 -->
						</view>
						<image v-if="PageCur=='tools'" src="../../static/tabBar/order_cur.png"></image>
						<image v-if="PageCur != 'tools'" src="../../static/tabBar/order.png"></image>
					</view>
					<view :class="PageCur=='tools'?'color_main':'text-gray'">工具箱</view>
				</view>

				<view class="action" @click="NavChange" data-cur="me">
					<view class='cuIcon-cu-image'>
						<view class="cu-tag badge">
							<!-- 红点 -->
						</view>
						<image v-if="PageCur=='me'" src="../../static/tabBar/me_cur.png"></image>
						<image v-if="PageCur != 'me'" src="../../static/tabBar/me.png"></image>
					</view>
					<view :class="PageCur=='me'?'color_main':'text-gray'">设置</view>
				</view>
				
			</view>
		</view>

	</view>
</template>

<script>
	import index from "./inedx.vue"; //首页
	import search from "./search.vue"; //技术视频
	import cases from "./main.vue"; //宅家学
	import me from "./me.vue"; //个人中心
	import tools from "./tools.vue"; //工具箱
	export default {
		components: {
			index,
			search,
			cases,
			me,
			tools
		},
		data() {
			return {
				PageCur: 'index',
				message: '2',
				openId: '',
				access_token: '',
				tip: "我是提示",
				duration: 1

			};
		},
		// 分享小程序
		onShareAppMessage(res) {
			return {
				title: '学技术·找案例，快来「前端铺子」吧！',
			};
		},
		onLoad() {
			wx.showShareMenu({
				withShareTicket: true
			})
		},
		onShareTimeline() {
			return {
				title: '学技术·找案例，快来「前端铺子」吧！',
			}
		},
		onLoad() {
			//获取退出时的tabbar记录
			// uni.getStorage({
			// 	key: 'PageCur',
			// 	success: function(res) {
			// 		that.PageCur = res.data;
			// 	},
			// 	fail: function(res) {}
			// });
		},
		methods: {
			ShowNews(e) {
				console.log(e)
				this.PageCur = e;
			},
			NavChange: function(e) {
				console.log(e.currentTarget.dataset.cur)

				this.PageCur = e.currentTarget.dataset.cur;

				if (this.PageCur == 'index') {
					// document.title = '首页'
				} else if (this.PageCur == 'component') {
					// document.title = '积分商城'
				} else if (this.PageCur == 'cases') {
					// document.title = '宅家学'
				} else if (this.PageCur == 'me') {
					// document.title = '个人中心'
				} else if (this.PageCur == 'tools') {
					//document.title = '工具箱'
				}

				// uni.setStorage({
				// 	key: 'PageCur',
				// 	data: this.PageCur,
				// 	success: function() {
				// 		console.log('保存成功！');
				// 	}
				// });
			},
			NavChange_xd: function() {
				uni.navigateTo({
					url: 'publish',
					animationType: 'slide-in-bottom',
					animationDuration: 200
				});
			}
		}
	}
</script>

<style lang="scss">
	.color_main {
		color: #000000;
		font-weight: 900;
	}

	.box {
		margin: 20upx 0;
	}

	.box view.cu-bar {
		margin-top: 20upx;
	}

	.logo_btn {
		width: 38*2rpx;
		height: 38*2rpx;
		position: absolute;
		z-index: 2;
		border-radius: 50%;
		top: -40rpx;
		left: 0rpx;
		right: 0;
		margin: auto;
		padding: 0;
	}

	.cu-bar.tabbar .action.add-action {
		padding-top: 56rpx !important;
	}
</style>
