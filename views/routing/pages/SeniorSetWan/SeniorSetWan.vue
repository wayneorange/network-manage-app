<template>
	<view>

		<view class="cu-container">
			<!-- WAN口设置 -->
			<view class="cu-item">				<text class='text-xl text-bold text-blue text-shadow title'>WAN口设置</text>
				<view class="content">
					<view class="form-item">WAN口连接类型：动态IP</view>
					<view class="margin-top-sm">
						<form>
							<view class="form-item">
								<label class="mtu-class">MTU:{{mtu}}</label>
								<input type="text" placeholder="请输入最大传输单元" class="input_style" v-model="inputcontent" />
							</view>
							<view class="margin-top-sm btns">
								<button size="mini" @click="submit">创建</button>
								<button size="mini" class="button_style" @click="reset">重置</button>
							</view>
						</form>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import request from '@/common/request.js';
	var _this;
	export default {
		data() {
			return {
				inputcontent: '', //输入内容
				current: 0,
				mtu: null,
				timer: null,
			};
		},
		onLoad() {
			_this = this;
			_this.getData();
		},
		methods: {
			getData() {
				console.log('数据加载');
				let opts = {
					url: '/dz/ICommond',
					method: 'get'
				};
				request.httpRequest(opts).then(res => {
					console.log(res);
					if (res.statusCode == 200) {
						_this.mtu = res.data.data[0].mtu;
						console.log(res.data.data[0].mtu);
					}
				});
			},
			postdata() {
				let popts = {
					url: '/dz/Setmtu',
					method: 'post'
				};
				request.httpRequest(popts, _this.inputcontent).then(res => {
					console.log(res);
					if (res.statusCode == 200) {

					}
				});
			},
			//提交按钮方法
			submit: function() {
				if (this.inputcontent != "") {
					_this.postdata();
					_this.getData();
					console.log(this.inputcontent);
				} else {
					uni.showModal({
						title: "提示",
						content: "输入的内容不能为空",
						showCancel: false
					})

				}
			},
			//重置按钮方法
			reset: function() {
				this.inputcontent = "";
			},
			beforeDestroy() {
				if (_this.timer) { //如果定时器还在运行 或者直接关闭，不用判断
					clearInterval(_this.timer); //关闭
				}
			},
		}
	};
</script>

<style lang="less" scoped>	.cu-container {		width: 100%;		padding: 20px;		background-color: #f1f5f9;				.cu-item {			width: 100%;			border: 1px solid #FFFFFF;			background-color: #fff;			border-radius: 5px;			margin-bottom: 20px;						.title {				display: inline-block;				width: 100%;								// height: 25px;				border-bottom: 1px solid #f1f5f9;				font-weight: 600;				padding: 10px;			}						.content {				padding: 0 20px 10px 20px;								.form-item {					display: flex;					align-items: center;					margin: 10px 0;				}				.mtu-class {					display: inline-block;				}								.btns {					margin-top: 100px;										button {						margin-right: 20px;					}				}			}		}	}
	// page {
	// 	height: 100%;
	// 	background-color: white;
	// }
</style>
