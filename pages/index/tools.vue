<!-- TabBar 文章资讯 -->
<template>
	<view>
		<cu-custom bgColor="bg-gradual-blue" :isBack="false">
			<!-- <block slot="backText">返回</block> -->
			<block slot="content">工具箱</block>
		</cu-custom>

		<!-- 导航栏 -->
		<view class="cu-list grid solids-bottom col-4 no-border">
			<view
				class="cu-item"
				v-for="(item, index) in categories"
				:key="index"
				:style="[{ animation: 'show ' + ((index + 1) * 0.2 + 1) + 's 1' }]"
				@click="goCategorieslist"
				:data-mid="item.mid"
			>
				<view :class="['cuIcon-' + item.cuIcon, 'text-' + item.color]">
					<view class="cu-tag badge" v-if="item.count != 0">
						<block v-if="item.badge != 1">{{ item.badge > 99 ? '99+' : item.badge }}</block>
					</view>
				</view>
				<text>{{ item.name }}</text>
			</view>
		</view>
	</view>
</template>

<script>
import request from '@/common/request.js';
export default {
	data() {
		return {
			categories: [
				{
					cuIcon: 'rankfill',
					color: 'red',
					badge: '测试',
					mid: '1',
					name: '九轴测试图'
				},
				{
					cuIcon: 'colorlens',
					color: 'orange',
					badge: '测试',
					mid: '2',
					name: '温湿度图'
				},
				{
					cuIcon: 'goodsnewfill',
					color: 'yellow',
					badge: '测试',
					mid: '3',
					name: '路由器测试'
				},
				{
					cuIcon: 'list',
					color: 'yellow',
					badge: '测试',
					mid: '4',
					name: '图表历史记录'
				},
				{
					cuIcon: 'male',
					color: 'yellow',
					badge: '测试',
					mid: '5',
					name: '实时环境历史记录'
				},
				{
					cuIcon: 'hotfill',
					color: 'yellow',
					badge: '测试',
					mid: '6',
					name: '实时姿态历史记录'
				},
				{
					cuIcon: 'hotfill',
					color: 'yellow',
					badge: '测试',
					mid: '7',
					name: '信号强度历史记录'
				}
			]
		};
	},
	mounted() {},
	methods: {
		goCategorieslist: function(e) {
			// console.log(e.currentTarget.dataset.mid)
			if (e.currentTarget.dataset.mid == 1) {
				// uni.navigateTo({
				// 	url: '../timeline?mid=' + e.currentTarget.dataset.mid
				// });
				uni.navigateTo({
					url: '../router/nineShaft'
				});
			} else if (e.currentTarget.dataset.mid == 2) {
				uni.navigateTo({
					url: '../router/tempetura'
				});
			} else if (e.currentTarget.dataset.mid == 3) {
				uni.navigateTo({
					url: '../router/routerDiagram'
				});
			} else if (e.currentTarget.dataset.mid == 4) {
				uni.navigateTo({
					url: '../router/test'
				});
			} else if (e.currentTarget.dataset.mid == 5) {
				uni.navigateTo({
					url: '../router/humidityHis'
				});
			} else if (e.currentTarget.dataset.mid == 6) {
				uni.navigateTo({
					url: '../router/nineShaftHis'
				});
			} else if (e.currentTarget.dataset.mid == 7) {
				uni.navigateTo({
					url: '../router/signalHis'
				});
			}
		}
	}
};
</script>

<style lang="scss" scoped>
.borderBottom {
	border-bottom: 1px solid #f2f2f2;
}
</style>
