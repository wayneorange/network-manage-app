<template>
	<view class="register-container">
		<h1>{{ title }}注册TP-Link ID</h1>
		<text class="tip"></text>
		<u-form :model="form" ref="uForm" :error-type="errorType">
			<u-form-item left-icon="account-fill" label="" prop="name">
				<u-input :placeholder="title" v-model.trim="form.name" />
			</u-form-item>
		</u-form>
		<u-button @click="nextStep" :custom-style="customButtonStyle">下一步</u-button>
		<view class="line">
			<u-icon @click="exchange" name="email-fill"></u-icon>
			<text @click="exchange">邮箱注册</text>
		</view>
		<u-toast ref="uToast" />
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '邮箱',
				form: {
					name: ''
				},
				rules: {
					name: [{
						required: true,
						message: '请输入手机号或邮箱',
						// 可以单个或者同时写两个触发验证方式 
						trigger: ['change'],
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
			},
			exchange: function() {
				this.title = this.title === '邮箱'?'手机': '邮箱';
			}
		},
		// 必须要在onReady生命周期，因为onLoad生命周期组件可能尚未创建完毕
		onReady() {
			this.$refs.uForm.setRules(this.rules);
		}
	}
</script>

<style lang="less">
	.register-container {
		height: 100%;
		width: 100%;
		padding: 20px;
		background-color: #fff;

		.tip {
			margin: 5px 0;
			display: inline-block;
			color: #cccccc;
		}
		
		.line {
			margin: 20px 0;
			display: inline-block;
			// color: #cccccc;
			
			.u-icon {
				margin-right: 10px;
			}
		}
	}
</style>

