<template>
	<view class="network-class-list">
		<view class="network-class-list-item" v-for="(item,index) in list" :key="index">
			<image :src="item.cover" mode="widthFix"></image>
			<view>
				<text>{{ item.name }}</text>
				<view class="network-class-list-item-href" @click="copy(index)">点击复制课程链接</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "class-list",	// 网课推荐的列表
		props: {
			list: {
				type: Array
			}
		},
		methods: {
			copy(index) {
				const href = this.list[index].path;
				uni.setClipboardData({
					data: href,
				})
			}
		}
	}
</script>

<style lang="scss">
	.network-class-list {
		.network-class-list-item {
			text-align: center;
			display: flex;
			flex-direction: column;
			align-items: center;
			background-color: #f7f3de;
			margin: 50rpx 0;
			border-radius: 12px;
			padding: 20rpx 0;
			position: relative;

			image {
				width: 600rpx;
				position: relative;
				border-radius: 12px;

				&::after {
					position: absolute;
					content: "";
					top: 0;
					left: 0;
					width: 100%;
					height: 100%;
					background-color: rgba(0, 0, 0, 0.3);
					border-radius: 15px;
					z-index: 2;
					pointer-events: none;
				}
			}



			&>view {
				margin-top: 20rpx;

				text {
					font-size: 36rpx;

				}

				.network-class-list-item-href {
					display: flex;
					align-items: center;
					justify-content: center;
					padding: 7rpx 40rpx;
					margin: 15rpx 0;
					background-color: #FFCD69;
					color: #fff;
					border-radius: 20px;
					font-size: 28rpx;
				}
			}
		}
	}
</style>
