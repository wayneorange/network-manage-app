<template>
	<view class="login-container">
		<view class="cancel" dir="rtl">
			<u-icon name="close" @click="cancel" color="#262626" size="28"></u-icon>
		</view>
		<view class="title">
			<h1>登录TP-Link ID</h1>
		</view>
		<view class="userForm">
			<u-form :model="form" ref="uForm">
				<u-form-item left-icon="account-fill" label="" style="white-space: nowrap;" prop="name">
					<u-input v-model="form.name" placeholder="TP-Link ID 为手机号或邮箱" />
				</u-form-item>
				<u-form-item left-icon="lock-fill" label="" prop="password">
					<u-input type="password" v-model="form.password" placeholder="请输入密码" />
				</u-form-item>
			</u-form>
		</view>
		<view class="lose-password" dir="rtl">
			<text @click="losePassword">忘记密码</text>
		</view>
		<view class="submit">
			<u-button :custom-style="customButtonStyle" @click="submit">登录</u-button>
		</view>
		<view class="toRegister">
			<text>没有TP-Link ID？立即注册</text>
		</view>

	</view>
</template>

<script>
import loginVue from './login.vue';
	export default {
		data() {
			return {
				form: {
					name: '',
					password: '',
				},
				rules: {
					name: [{
						required: true,
						message: '请输入账号名称',
						// 可以单个或者同时写两个触发验证方式 
						trigger: ['change'],
					}],
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
			}
		},
		methods: {
			submit() {
				this.$refs.uForm.validate(valid => {
					if (valid) {
						uni.switchTab({							url: '/views/state/state'						});
					} else {
						console.log('验证失败');
					}
				});
			},
			losePassword: function() {
				uni.navigateTo({
					url: '/views/losePassword/yanzheng/yanzheng'
				});
			},
			cancel() {
				console.log('退出登录')
			},
		},
		// 必须要在onReady生命周期，因为onLoad生命周期组件可能尚未创建完毕
		onReady() {
			this.$refs.uForm.setRules(this.rules);
		}
	}
</script>

<style lang="less">
	.login-container {
		height: 100%;
		width: 100%;
		background-color: #fff;
		padding: 20px;

		.cancel {
			.u-icon {
				// margin: 20px;
			}
		}

		.title {
			margin: 10px 0;
		}

		.userForm .u-form-item {
			white-space: nowrap;
		}

		.lose-password {
			margin: 20px 0;
		}

		.submit {
			width: 100%;
			margin-bottom: 20px;
		}

		.toRegister {
			width: 100%;
			display: flex;
			justify-content: center;
			color: #8f9eaa;
		}



	}
</style>
