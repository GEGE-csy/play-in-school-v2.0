<template>
	<view class="more-finding">
		<!-- 瀑布流布局列表 -->
		<view class="more-finding-container">
			<view class="more-finding-list">
				<view class="more-finding-list-item" v-for="(item, index) in list" :key="index"
					@click="itemClick(item.id)">
					<image :src="item.path" mode="widthFix"></image>
					<view class="more-finding-list-content">
						<!-- 这是没有高度的父盒子（下半部分） -->
						<view class="more-finding-list-title">
							{{ item.content }}
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default { // share-更多发现
		data() {
			return {
				list: [],
			};
		},
		methods: {
			getList() {
				uni.showLoading({
					title: '加载中'
				})
				this.$request({
					url: '/common/share/get',
				}).then(res => {
					uni.hideLoading();
					if (res.data.code === 200) {
						this.list = res.data.data;
					} else {
						uni.showToast({
							icon: 'error',
							title: '加载失败!'
						})
					}
				}, err => {
					uni.hideLoading();
					uni.showToast({
						icon: 'error',
						title: '加载失败!'
					})
				})
			}
		},
		created() {
			this.getList();
		},
		mounted() {
			uni.$on('success-add-finding', () => {
				this.getList();
			})
		}
	};
</script>

<style lang="scss">
	//瀑布流
	.more-finding-container {
		padding: 40rpx 60rpx;
	}

	.more-finding-tip {
		text-align: center;
		margin: 50rpx;
	}

	.more-finding-list {
		column-count: 2;
		column-gap: 20rpx;
	}

	.more-finding-list-item {
		overflow: hidden;
		background-color: #fff;
		break-inside: avoid;
		/*避免在元素内部插入分页符*/
		box-sizing: border-box;
		margin-bottom: 20rpx;

		image {
			width: 330rpx !important;
			margin-bottom: 20rpx;
		}

	}

	.more-finding-list-content {
		padding-left: 20rpx;
		padding-right: 20rpx;
		font-size: 24rpx;
		padding-bottom: 22rpx;

		.more-finding-list-title {
			font-size: 24rpx;
			line-height: 43rpx;
			color: #000000;
		}
	}
</style>
