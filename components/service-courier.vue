<template>
	<view class="service-courier">
		<view class="service-courier_item" v-for="(item,index) in itemList" :key="index">
			<view class="service-courier_text">
				{{item.title}}
			</view>
			<image :src="item.src" mode="widthFix"></image>
			<view class="service-courier_detail">{{item.text}}</view>
		</view>
	</view>
</template>

<script>
	export default {
		name:"service-courier",
		props: {
			itemList: {
				type: Array,
			}
		}
	}
</script>

<style lang="scss">
.service-courier {
		padding-top: 20rpx;
		margin: 0 20rpx;

		.service-courier_item {
			text-align: center;
			margin-bottom: 40rpx;
			background-color: #FFE9D3;
			border-radius: 8px;
			padding-bottom: 30rpx;
			
			.service-courier_text {
				float: left;
				padding-left: 40rpx;
				margin: 30rpx 0;
			}

			image {
				width: 90%;
			}
			.service-courier_detail {
				width: 80%;
				margin: 0 auto;
			}
		}
	}
</style>
