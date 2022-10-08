<template>
	<view class="networkManage-containwer">
		<view class="line">
			<view class="line-item">
				<text>上网设置</text>
				<view class="right" @click="selectNet">
					<text>{{ selectedNet.label }}</text>
					<u-icon name="arrow-right"></u-icon>
					<u-select v-model="netShow" :list="netList" :default-value="[selectEqIndex]"
						@confirm="confirmSelect"></u-select>
				</view>
			</view>
		</view>
		<view class="line">
			<text class="title">{{ showData.name }}</text>
			<view class="con" v-for="(item, i) in showData.list || []" :key="i">
				<text class="name">{{ item.name }}</text>
				<text>: </text>
				<text class="value">{{ item.value }}</text>
			</view>
		</view>
		<view class="line">
			<view class="line-item" @click="advanceSetting">
				<text>高级设置</text>
				<view class="right">
					<u-icon name="arrow-right"></u-icon>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				netList: [{
						value: 'zd',
						label: '自动获取IP地址'
					},
					{
						value: 'sd',
						label: '手动设置Ip地址'
					}
				],
				netShow: false,
				selectedNet: {
					value: 'zd',
					label: '自动获取IP地址'
				},
				showData: {
					name: '正常上网',
					list: [{
							name: 'Ip',
							value: '192.168.1.2'
						},
						{
							name: '子关掩码',
							value: '255.255.255.0'
						},
						{
							name: '网关地址',
							value: '192.168.1.1'
						},
						{
							name: '首选DNS',
							value: '192.168.1.2'
						},
						{
							name: '备选DNS',
							value: '0.0.0.0'
						}
					]
				}
			}
		},
		computed: {
			selectEqIndex() {
				const index = this.netList.findIndex(o => {
					return o.value === this.selectedNet.value
				});
				return index >= 0 ? index : 0;
			},
		},
		methods: {
			advanceSetting: function() {
				console.log('高级设置');
			},
			confirmSelect(e) {
				this.selectedNet = e[0];
			},
			selectNet() {
				this.netShow = true
			}
		}
	}
</script>

<style lang="less">
	.networkManage-containwer {
		width: 100%;
		background-color: #f2f2f2;

		.line {
			background-color: #fff;
			margin-top: 20px;
			padding: 10px 20px;

			.title {
				display: inline-block;
				margin: 10px 0;
				color: #4c9ad4;
			}

			.line-item {
				height: 100%;
				width: 100%;
				display: flex;
				align-items: center;
				justify-content: space-between;

				.right {
					color: #7f7f7f;
				}
			}
		}
	}
</style>
