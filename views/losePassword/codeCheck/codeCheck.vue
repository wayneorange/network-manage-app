<template>
	<view class="codeCheck-container">
		<h1>输入验证码</h1>
		<text class="tip">我们已向 {{ name }} 发送验证码，请查看后输入验证码</text>
		<u-message-input mode="bottomLine" :focus="true" :breathe="true" :maxlength="6" @change="change"></u-message-input>
		<u-button @click="submit" class="submit-btn" :class="isFinished?'': 'isDisabled'" :custom-style="customButtonStyle">确认</u-button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				name: '',
				code: '',
				customButtonStyle: {
					backgroundColor: '',
					border: 'none',
					cursor: 'not-allowed',
					pointerEvents: 'none'
				},
				isFinished: false
			};
		},
		methods: {
			change: function(e) {
				this.code = e
				this.isFinished = e.length === 6;
				this.customButtonStyle.backgroundColor = this.isFinished ? '#a0ceff': '';
				this.customButtonStyle.cursor = this.isFinished ? 'default': 'not-allowed';
				this.customButtonStyle.pointerEvents = this.isFinished ? 'auto': 'none';
				
 			},
			submit: function() {
				console.log('提交验证验证码', this.code);
				uni.navigateTo({
					url: '/views/losePassword/rePassword/rePassword'
				});
			}
			
		},
		onLoad: function (options) {
			this.name = options.name || ''
		}
	}
</script>

<style lang="less">
.codeCheck-container {
	height: 100%;
	width: 100%;
	padding: 20px;
	background-color: #fff;
	
	.tip {
		margin: 5px 0;
		display: inline-block;
		color: #cccccc;
	}
	
	.submit-btn {
		margin: 20px 0;
	}
}
</style>
