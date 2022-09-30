<template>
	<view class="yanzheng-container">
		<h1>验证TP-Link ID</h1>
		<text class="tip">请输入注册的手机号或者邮箱</text>
		<u-form :model="form" ref="uForm" :error-type="errorType">
			<u-form-item left-icon="account-fill" label="" prop="name">
				<u-input placeholder="请输入注册的手机号或邮箱" v-model.trim="form.name" />
			</u-form-item>
		</u-form>
		<u-button @click="nextStep" :custom-style="customButtonStyle">下一步</u-button>
		<u-toast ref="uToast" />
	</view>
</template>

<script>
	export default {
		data() {
			return {
				form: {
					name: ''
				},
				rules: {
					name: [{
						required: true,
						message: '请输入手机号或邮箱',
						// 可以单个或者同时写两个触发验证方式 
						trigger: ['change', 'blur'],
					}]
				},
				errorType: ['message'],
				customButtonStyle: {
					backgroundColor: '#a0ceff',
					border: 'none'
				}
			};
		},
		methods: {
			nextStep() {
				if (this.form.name === '') {
					return this.$refs.uToast.show({
						position: 'top',
						title: '请输入手机号或邮箱',
						type: 'error',
					})
				}
				console.log('提交并验证信息: ', this.form);
				uni.navigateTo({
					url: '/views/losePassword/codeCheck/codeCheck?name=' + this.form.name
				});
			}
		},
		// 必须要在onReady生命周期，因为onLoad生命周期组件可能尚未创建完毕
		onReady() {
			this.$refs.uForm.setRules(this.rules);
		}
	}
</script>

<style lang="less">
	.yanzheng-container {
		height: 100%;
		width: 100%;
		padding: 20px;
		background-color: #fff;

		.tip {
			margin: 5px 0;
			display: inline-block;
			color: #cccccc;
		}
	}
</style>
