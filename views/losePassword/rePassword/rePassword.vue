<template>
	<view class="rePassword-container">
		<h1>设置密码</h1>
		<text class="tip">6 ~ 32 个字符</text>
		<u-form :model="form" ref="uForm">
			<u-form-item left-icon="lock-fill" label="" prop="password">
				<u-input type="password" v-model.trim="form.password" />
			</u-form-item>
		</u-form>
		<u-button class="submit-btn" @click="submit" :custom-style="customButtonStyle">完成</u-button>
		<u-toast ref="uToast" />
	</view>
</template>

<script>
	export default {
		data() {
			return {
				form: {
					password: ''
				},
				rules: {
					password: [{
						required: true,
						message: '请输入密码',
						// 可以单个或者同时写两个触发验证方式 
						trigger: ['change'],
					}, {
						min: 6,
						message: '密码不能少于6个字符',
						trigger: 'change'
					}]
				},
				customButtonStyle: {
					backgroundColor: '#a0ceff',
					border: 'none'
				}
			};
		},
		methods: {
			submit: function() {
				console.log('提交修改后的密码', this.form.password);
				
				if(this.form.password === '') {
					return this.$refs.uToast.show({
						title: '请输入密码',
						type: 'error',
					})
				}
				this.$refs.uToast.show({
					title: '修改成功！',
					type: 'success',
					duration: 1000,
					url: '/views/login/login'
				})
			}
		},
		// 必须要在onReady生命周期，因为onLoad生命周期组件可能尚未创建完毕
		onReady() {
			this.$refs.uForm.setRules(this.rules);
		}
	}
</script>

<style lang="less">
	.rePassword-container {
		height: 100%;
		width: 100%;
		background-color: #fff;
		padding: 20px;

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
