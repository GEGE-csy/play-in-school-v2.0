<template>
	<view class="lost-and-find">
		<picker mode="selector" :range="type" @change="bindTypeChange" :value="typeIndex">
			<text>{{type[typeIndex]}}</text>
			<image src="https://play-in-gdufs.oss-cn-beijing.aliyuncs.com/%E9%80%89%E6%8B%A9%E6%8C%89%E9%92%AE.png"
				mode="widthFix"></image>
		</picker>
		<view class="lost-and-find-detail">
			<view class="lost-and-find-detail_item" @click="goDetail(index)" v-for="(item,index) in findType" :key="index">
				<image :src="item.icon" mode="widthFix"></image>
				<view class="lost-and-find-detail_title">
					{{ item.text }}
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default { // service-失物招领
		name: "lost-and-find",
		data() {
			return {
				typeIndex: 0,
				type: ['找物品', '找失主'],
				findType: [{
						icon: 'https://play-in-gdufs.oss-cn-beijing.aliyuncs.com/dianzishebei.png',
						text: '电子设备',
					},
					{
						icon: 'https://play-in-gdufs.oss-cn-beijing.aliyuncs.com/qiapian.png',
						text: '校卡',
					},
					{
						icon: 'https://play-in-gdufs.oss-cn-beijing.aliyuncs.com/a-baoxianyusanyujuxiayu.png',
						text: '雨具',
					},
					{
						icon: 'https://play-in-gdufs.oss-cn-beijing.aliyuncs.com/yifu.png',
						text: '衣物',
					},
					{
						icon: 'https://play-in-gdufs.oss-cn-beijing.aliyuncs.com/gengduo1.png',
						text: '其他',
					},
				]
			};
		},
		methods: {
			bindTypeChange(e) {
				this.typeIndex = e.detail.value;
			},
			goDetail(index) {
				uni.navigateTo({
					url: `/pages/lost-find-list/lost-find-list?type=${index}&category=${this.typeIndex}`
				})
			}
		}
	}
</script>

<style lang="scss">
	.lost-and-find {
		picker {
			color: #919191;
			text-align: right;
			padding-right: 50rpx;
			margin-bottom: 20rpx;
			text {
				margin-right: 10rpx;
			}
			image {
				width: 20rpx;
			}
		}

		.lost-and-find-detail {
			display: flex;
			flex-wrap: wrap;

			.lost-and-find-detail_item {
				background-color: #FFE9D3;
				display: flex;
				padding: 12rpx 24rpx;
				border-radius: 18px;
				width: 220rpx;
				height: 200rpx;
				flex-direction: column;
				justify-content: center;
				align-items: center;
				margin: 20rpx;

				.lost-and-find-detail_title {
					color: #FFAA54;
				}
				image {
					width: 90rpx;
					margin-bottom: 10rpx;
				}

				&:nth-child(4) {
					image {
						width: 85rpx;
						margin-top: 10rpx;
						margin-bottom: 10rpx;
					}
				}

				&:last-child {
					width: 530rpx;
					flex-direction: row;

					image {
						margin-right: 10rpx;
					}

					.lost-and-find-detail_title {
						width: 70rpx;
						color: #FFAA54;
					}
				}

			}
		}
	}
</style>
