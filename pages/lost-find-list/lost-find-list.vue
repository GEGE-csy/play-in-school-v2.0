<template>
	<view class="lost-find-list">
		<view class="lost-find-list-container" v-if="isShowList">
			<view class="lost-find-list-list">
				<view class="lost-find-list-list-item" v-for="(item, index) in list" :key="index">
					<image :src="item.path" mode="widthFix"></image>
					<view class="lost-find-list-list-content">
						<!-- 这是没有高度的父盒子（下半部分） -->
						<view class="lost-find-list-list-title">
							{{ item.content }}
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="lost-find-list-tip" v-else>
			暂无更多信息
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [],
				type: -1, // 0电子设备 1校卡 2雨具 3衣物 4其他 
				category: -1, // 0找物品 1找失主
				isShowList: false,
			}
		},
		methods: {
			getList() {
				uni.showLoading({
					title: '加载中'
				})
				this.$request({
					url: '/lost/list',
					data: {
						itemType: +this.type + 1,
						searchType: +this.category + 1
					}
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
		watch: {
			list(res) {
				this.isShowList = res.length !== 0 ? true : false;
			}
		},
		onLoad(options) {
			this.type = options.type;
			this.category = options.category;
		},
		onShow() {
			this.getList();
		},
		onPullDownRefresh() {
			this.getList();
			setTimeout(function() {
				uni.stopPullDownRefresh();
			}, 1000);
		}

	}
</script>

<style lang="scss">
	.lost-find-list-container {
		padding: 40rpx 60rpx;
	}


	.lost-find-list-list {
		column-count: 2;
		column-gap: 20rpx;
	}

	.lost-find-list-list-item {
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

	.lost-find-list-list-content {
		padding-left: 20rpx;
		padding-right: 20rpx;
		font-size: 24rpx;
		padding-bottom: 22rpx;

		.lost-find-list-list-title {
			font-size: 24rpx;
			line-height: 43rpx;
			color: #000000;
		}
	}
</style>
