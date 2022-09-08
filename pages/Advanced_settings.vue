<template>
<view>
	<cu-custom bgColor="bg-gradual-blue" :isBack="true">
		<block slot="backText"></block>
		<block slot="content">{{'高级设置'}}</block>
	</cu-custom>
	
	<view class="cu-timeline">
		<!-- WAN口设置 -->
		<view class="cu-time">
			<text class='cuIcon-selection text-white text-lg bg-blue round padding-xs'></text>
			<text class='text-xl margin-left'>WAN口设置</text>
		</view>
		<view class="cu-item text-blue">
			<view class="content">
				<view>WAN口连接类型：动态IP</view>
				<view class="margin-top-sm">
					<form>
						<view><view>MTU:{{mtu}}</view>
						<input type="text" placeholder="请输入最大传输单元" class="input_style" v-model="inputcontent"/>
						</view>
						<view class="margin-top-sm">	
							<button size="mini" @click="submit" >创建</button>
							<button size="mini" class="button_style" @click="reset" >重置</button>
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
	export default{
		data(){
			return{
				inputcontent:'', //输入内容
				current: 0,
				mtu:null,
				timer: null,
			};
		},
		onLoad() {
			_this=this;
			_this.getData();
		},
		methods:{
			getData() {
				console.log('数据加载');
				let opts = {
					url: '/dz/ICommond',
					method: 'get'
				};
				request.httpRequest(opts).then(res => {
					console.log(res);
					if (res.statusCode == 200) {
						_this.mtu=res.data.data[0].mtu;
						console.log(res.data.data[0].mtu);
					}	
				});
			},
			postdata(){
				let popts = {
					url: '/dz/Setmtu',
					method: 'post'
				};
				request.httpRequest(popts,_this.inputcontent).then(res => {
					console.log(res);
					if (res.statusCode == 200) {
						
					}
				});
			},		
		//提交按钮方法
			submit:function(){
				if(this.inputcontent!="")
				{   _this.postdata();
				    _this.getData();
				    console.log(this.inputcontent);
				}
				else
				{
					uni.showModal(
					{
						title:"提示",
						content:"输入的内容不能为空",
						showCancel:false
					})
							
				}
			},
			//重置按钮方法
			reset:function(){
				this.inputcontent="";
			},
			beforeDestroy() {
				if (_this.timer) { //如果定时器还在运行 或者直接关闭，不用判断
					clearInterval(_this.timer); //关闭
				}
			},	
		}
	};
</script>

<style>
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