<template>
	<view>

		<!-- WAN口状态 -->
		<view class="cu-container">
			<view class="cu-item">				<text class='text-xl text-bold text-blue text-shadow title'>WAN口状态</text>
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
			<view class="cu-item">				<text class='text-xl text-bold text-blue text-shadow title'>5G模块状态</text>
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
			<view class="cu-item">				<text class='text-xl text-bold text-blue text-shadow title'>信息</text>
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
					SubNetMask: null,
					TxPackage: null,
					RxPackage: null,
					MTU: null,
					MAC: null,
					ConnectionTime: null,
					//5G模块状态
					FG_ConnectionState: null,
					FG_SIMState: null,
					FG_InternetIF: null,
					FG_CSIIF: null,
					FG_ModuleTime: null,
					FG_TMP: null,
					FG_RGMII_Mac: null,
					FG_SignalIntensity: null,
					//信息
					In_RunTime: null,
				},

				date: new Date().toISOString(),
				swiperCurrent: 0,
				dx: 0,

			};
		},
		onLoad(option) {},
		mounted() {
			_self = this;
			setInterval(function() {
				this.date = Date.parse(new Date());
			}, 1000);
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
					if (res.statusCode == 200) {
						// console.log(res);
						//WAN口状态
						this.sys.WAN = res.data.data[0].ipwan;
						console.log(res.data.data[0].ipwan);
						// console.log(res.data.data[0]);
						this.sys.SubNetMask = res.data.data[0].subnetMask;
						console.log(res.data.data[0].subnetMask);

						this.sys.TxPackage = res.data.data[0].txPackage;
						this.sys.RxPackage = res.data.data[0].rxPackage;
						this.sys.MTU = res.data.data[0].mtu;
						this.sys.MAC = res.data.data[0].mac;
						this.sys.ConnectionTime = res.data.data[0].date;
						//5G模块
						this.sys.FG_ConnectionState = res.data.data[2].at_QNWINFO;
						this.sys.FG_SIMState = res.data.data[2].qsimstat;
						this.sys.FG_RGMII_Mac = res.data.data[2].at_mac_address.replaceAll("\"", "").replaceAll(
							"mac_address,", "").match("(?<=QETH:).*");
						this.sys.FG_CSIIF = res.data.data[2].at_QNWCFG.replaceAll("\"", "").replaceAll("lte_csi,",
							"").match("(?<=QNWCFG:).*");
						this.sys.FG_SignalIntensity = res.data.data[2].at_csq.match("(?<=CSQ:).*");
						this.sys.FG_ModuleTime = res.data.data[2].at_CCLK.replaceAll("\"", "").match(
							"(?<=CCLK:).*");
						this.sys.FG_TMP = res.data.data[2].at_QTEMP.replaceAll("\"", "").replaceAll("qfe_wtr_pa0,",
							"").match("(?<=QTEMP:).*") + "℃";
						//信息
						_self.sys.In_RunTime = res.data.data[1].systemruntime;
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

<style lang="scss" scoped>	.cu-container {		width: 100%;		padding: 20px;		background-color: #f1f5f9;				.cu-item {			width: 100%;			border: 1px solid #FFFFFF;			background-color: #fff;			border-radius: 5px;			margin-bottom: 20px;						.title {				display: inline-block;				width: 100%;								// height: 25px;				border-bottom: 1px solid #f1f5f9;				font-weight: 600;				padding: 10px;			}						.content {				padding: 0 20px 10px 20px;			}		}	}
	page {
		height: 100%;
		background-color: white;
	}
</style>
