<template>
	<view>
		<cu-custom bgColor="bg-gradual-blue" :isBack="true">
			<block slot='backText'>返回</block>
			<block slot="content">{{'系统状态'}}</block>
		</cu-custom>

		<!-- WAN口状态 -->
		<view class="cu-timeline">
			<view class="cu-time">
				<text class='cuIcon-selection text-white text-lg bg-blue round padding-xs'></text>
				<text class='text-xl margin-left'>WAN口状态</text>
			</view>
			<view class="cu-item text-blue">
				<view class="content">
					<view class="margin-top">连接状态：已连接<text id="W_connection state"></text></view>
					<view class="margin-top-sm">WAN口IP：{{sys.WAN}}</view>
					<view class="margin-top-sm">子网掩码：{{sys.SubNetMask}}</view>
					<view class="margin-top-sm">连接方式：动态IP</view>
					<view class="margin-top-sm">物理地址：{{sys.MAC}}</view>
					<view class="margin-top-sm">已发包（TX）：{{sys.TxPackage}}</view>
					<view class="margin-top-sm">已收包（RX）：{{sys.RxPackage}}</view>
					<view class="margin-top-sm">最大传输单元（MTU）：{{sys.MTU}}</view>
					<view class="margin-top-sm">连接时间：{{sys.ConnectionTime}}</view>
				</view>
			</view>

			<!--5G模块状态-->
			<view class="cu-time">
				<text class='cuIcon-selection text-white text-lg bg-blue round padding-xs'></text>
				<text class='text-xl margin-left'>5G模块状态</text>
			</view>
			<view class="cu-item text-blue">
				<view class="content">
					<view class="margin-top">连接状态：已连接</view>
					<view class="margin-top-sm">SIM卡状态：已插入</view>
					<view class="margin-top-sm">互联网信息：oService</view>
					<view class="margin-top-sm">CSI信息：{{sys.FG_CSIIF}}</view>
					<view class="margin-top-sm">模块时间：{{sys.FG_ModuleTime}}</view>
					<view class="margin-top-sm">模块温度：{{sys.FG_TMP}}</view>
					<view class="margin-top-sm">RGMII MAC地址：{{sys.FG_RGMII_Mac}}</view>
					<view class="margin-top-sm">信号强度：{{sys.FG_SignalIntensity}}</view>
				</view>
			</view>
			<!-- 信息-->
			<view class="cu-time">
				<text class='cuIcon-selection text-white text-lg bg-blue round padding-xs'></text>
				<text class='text-xl margin-left'>信息</text>
			</view>
			<view class="cu-item text-blue">
				<view class="content">
					<view class="margin-top">运行时间：{{sys.In_RunTime}}</view>
					<view class="margin-top-sm">系统时间：{{dateFormat(date)}}</view>
					<view class="margin-top-sm">系统版本：SR1.1</view>

				</view>
			</view>
		</view>
	</view>
</template>


<script>
	import choiceSelected from '@/components/selected/ChoiceSelected.vue';
	import request from '@/common/request.js';
	var _self;
	export default {
		data() {
			return {
				arr: {},
				sys: {
					//WAN口状态
					WAN: null,
					SubNetMask:null,
					TxPackage: null,
					RxPackage:null,
					MTU:null,
					MAC:null,
					ConnectionTime:null,
					//5G模块状态
					FG_ConnectionState:null,
					FG_SIMState:null,
					FG_InternetIF:null,
					FG_CSIIF:null,
					FG_ModuleTime:null,
					FG_TMP:null,
					FG_RGMII_Mac:null,
					FG_SignalIntensity:null,
					//信息
					In_RunTime:null,
				},
				
				date: new Date().toISOString(),
				swiperCurrent: 0,
				dx: 0,
				
			};
		},
		onLoad(option) {
		},
		mounted() {
			_self = this;
			setInterval(function() {this.date = Date.parse(new Date());}, 1000);
			_self.getData();
		},
		methods: {
			// 通过后台获取数据
			getData() {
				console.log('数据加载');
				let opts = {
					url: '/dz/ICommond',
					method: 'get'
				};
				uni.showLoading({
					title: '加载中'
				});
				request.httpRequest(opts).then(res => {
					console.log(res);
					uni.hideLoading();
					if (res.statusCode ==200) {
						// console.log(res);
						//WAN口状态
						this.sys.WAN=res.data.data[0].ipwan;
						console.log(res.data.data[0].ipwan);
						// console.log(res.data.data[0]);
						this.sys.SubNetMask=res.data.data[0].subnetMask;
						console.log(res.data.data[0].subnetMask);
						
						this.sys.TxPackage=res.data.data[0].txPackage;
						this.sys.RxPackage=res.data.data[0].rxPackage;
						this.sys.MTU=res.data.data[0].mtu;
						this.sys.MAC=res.data.data[0].mac;
						this.sys.ConnectionTime=res.data.data[0].date;
						//5G模块
						this.sys.FG_ConnectionState=res.data.data[2].at_QNWINFO;
						this.sys.FG_SIMState=res.data.data[2].qsimstat;
						this.sys.FG_RGMII_Mac=res.data.data[2].at_mac_address.replaceAll("\"","").replaceAll("mac_address,","").match("(?<=QETH:).*");
						this.sys.FG_CSIIF=res.data.data[2].at_QNWCFG.replaceAll("\"","").replaceAll("lte_csi,","").match("(?<=QNWCFG:).*");
						this.sys.FG_SignalIntensity=res.data.data[2].at_csq.match("(?<=CSQ:).*");
						this.sys.FG_ModuleTime=res.data.data[2].at_CCLK.replaceAll("\"","").match("(?<=CCLK:).*");
						this.sys.FG_TMP=res.data.data[2].at_QTEMP.replaceAll("\"","").replaceAll("qfe_wtr_pa0,","").match("(?<=QTEMP:).*")+"℃";
						//信息
						_self.sys.In_RunTime=res.data.data[1].systemruntime;
					} else {}
				});
			},
			//时间显示
			dateFormat(time) {
							let date = new Date(time);
							let year = date.getFullYear();
							// 在日期格式中，月份是从0开始的，因此要加0，使用三元表达式在小于10的前面加0，以达到格式统一  如 09:11:05
							let month = date.getMonth() + 1 < 10 ? "0" + (date.getMonth() + 1) : date.getMonth() + 1;
							let day = date.getDate() < 10 ? "0" + date.getDate() : date.getDate();
							let hours = date.getHours() < 10 ? "0" + date.getHours() : date.getHours();
							let minutes = date.getMinutes() < 10 ? "0" + date.getMinutes() : date.getMinutes();
							let seconds = date.getSeconds() < 10 ? "0" + date.getSeconds() : date.getSeconds();
							// 拼接
							return year + "-" + month + "-" + day + " " + hours + ":" + minutes + ":" + seconds;
							// return year + "-" + month + "-" + day;
						},
			// tab栏切换
			change(index) {
				this.swiperCurrent = index;
			},
			transition({
				detail: {
					dx
				}
			}) {
				this.$refs.tabs.setDx(dx);
			},
			animationfinish({
				detail: {
					current
				}
			}) {
				this.$refs.tabs.setFinishCurrent(current);
				this.swiperCurrent = current;
				this.current = current;
			},
		}
	};
</script>

<style lang="scss" scoped>
	/* #ifndef H5 */
	page {
		height: 100%;
		background-color: #f2f2f2;
	}

	/* #endif */
</style>

<style lang="scss" scoped>
	.cu-timeline .cu-time {
		width: 100%;
		text-align: left;
		padding: 20rpx 0 20rpx 37rpx;
		font-size: 26rpx;
		color: #888;
		display: block;
	}

	.text-red,
	.line-red,
	.lines-red {
		color: #FF3434;
	}

	.margin-avatar {
		margin-left: -15rpx;
	}

	.margin-avatar-bottom {
		margin-bottom: 150rpx;
	}

	.line-blue-tuniao::after {
		border-color: #0070ff !important;
		color: #0070ff;
	}

	.resume {
		/* background: #f1f1f1; */
		padding-top: 10rpx;
		border-radius: 6rpx;
		display: block;
		color: #666;
		line-height: 1.6;
	}

	.resume+.resume {
		margin-top: 20rpx;
	}

	.resume2 {
		padding-top: 10rpx;
		border-radius: 6rpx;
		display: block;
		color: #666;
		line-height: 1.6;
	}

	.edit {
		position: fixed;
		width: 100rpx;
		height: 100rpx;
		bottom: 250rpx;
		right: 30rpx;
		z-index: 1;
		opacity: 0.8;
		border: 1px solid #189eff;
		border-radius: 100rpx;
		box-shadow: 0rpx 0rpx 6rpx rgba(24, 158, 255, 1);
		padding: 20rpx;
	}

	.love {
		position: fixed;
		width: 100rpx;
		height: 100rpx;
		bottom: 550rpx;
		right: 30rpx;
		z-index: 1024;
		opacity: 0.8;
		border: 1px solid #189eff;
		border-radius: 100rpx;
		box-shadow: 0rpx 0rpx 6rpx rgba(24, 158, 255, 1);
		padding: 20rpx;
	}

	.bg-img-cont {
		background-size: cover;
		background-position: center;
		background-repeat: no-repeat;
		height: 350rpx;
	}

	.share-png {
		width: 100rpx;
		height: 100rpx;
		margin: 0 auto;
	}

	.share-wechat {
		width: 35rpx;
		height: 35rpx;
		margin: 0 10rpx -4rpx 0;
		opacity: 0.5;
	}

	.button-no::after {
		border: none;
	}

	.title-pyq {
		background-image: -webkit-linear-gradient(0deg, #1b6cff, #1ca5ff);
		/*1B6CFF 1B42FF*/
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		opacity: 0.5;
	}

	.edit-fixed {
		position: fixed;
		width: 100%;
		bottom: 0;
		z-index: 1024;
		box-shadow: 0 1rpx 6rpx rgba(0, 0, 0, 0.1);
	}

	.button-no {
		border: none;
		width: 100%;
		height: 100%;
		background-color: rgba(0, 0, 0, 0);
	}

	.centre {
		text-align: center;
		margin: 200rpx auto;
		font-size: 32rpx;

		image {
			width: 300rpx;
			border-radius: 50%;
			margin: 0 auto;
		}

		.tips {
			font-size: 24rpx;
			color: #999999;
			margin-top: 20rpx;
		}

		.btn {
			margin: 80rpx auto;
			width: 200rpx;
			border-radius: 32rpx;
			line-height: 64rpx;
			color: #ffffff;
			font-size: 26rpx;
			background: linear-gradient(270deg, #1cbbb4 0%, #0081ff 100%);
		}
	}

	.wrap {
		display: flex;
		flex-direction: column;
		height: calc(100vh - var(--window-top));
		width: 100%;
	}

	.swiper-box {
		flex: 1;
	}

	.swiper-item {
		height: 100%;
	}
	.input_style{
		background-color: #ffffff;
		width: 150px;
		
	}
	.button_style{
		margin-left: 20px;
	}
	  .selected-view {
	        width: 80%;
	        margin-top: 15rpx;
	    }
	
	    .content {
	        display: flex;
	        flex-direction: column;
	        align-items: center;
	        width: 100%;
	        height: 100%;
	        background-color: white;
	    }
	
	    page {
	        height: 100%;
	        background-color: white;
	    }
</style>
