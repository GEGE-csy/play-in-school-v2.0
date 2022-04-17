<template>
	<view class="service">
		<view class="service-title">
			<view class="service-title_text">
				校园服务
			</view>
			<view class="service-title-img">
				<image :src="titleImg" mode="widthFix"></image>
			</view>
		</view>
		<!-- 切换校园充值、快递寄取、失物招领 -->
		<view class="service-head">
			<view @click="switchType(index)" 
						class="service-head_item" 
						v-for="(item,index) in title" 
						:key="index"
						:class="{activeService: index === activeIndex}">
				{{item}}
			</view>
		</view>
		<view class="service-content">
			<view class="service-content_detail">
				<view v-if="activeIndex === 0">
					<service-pay></service-pay>
				</view>
				<view v-else-if="activeIndex === 1">
					<service-courier :itemList="courierList"></service-courier>
				</view>
				<view v-else>
					<lost-and-find></lost-and-find>
				</view>
			</view>
		</view>
		<view class="service-add" @click="addObject" v-show="activeIndex === 2">
			<add-service-object v-show="isShowAdd" @close="close"></add-service-object>
			<image src="https://play-in-gdufs.oss-cn-beijing.aliyuncs.com/tianjia.png" mode="widthFix"></image>
		</view>
	</view>
</template>

<script>
	import servicePay from '../../components/service-pay.vue';
	import serviceCourier from '../../components/service-courier.vue';
	import lostAndFind from '../../components/lost-and-find.vue';
	import addServiceObject from '../../components/add-service-object.vue';
	import {
		courierList,
		findList
	} from '../../data/service.js'
	export default {
		data() {
			return {
				title: ['校园充值', '快递寄取', '失物招领'],
				titleImg: 'https://play-in-gdufs.oss-cn-beijing.aliyuncs.com/temp/%E5%AE%9A%E4%BD%8D%E4%B8%A2%E5%A4%B1-3.png',
				activeIndex: 0,
				courierList,
				findList,
				isShowAdd: false,
			}
		},
		components: {
			servicePay,
			serviceCourier,
			lostAndFind,
			addServiceObject
		},
		methods: {
			switchType(index) {
				this.activeIndex = index;
			},
			addObject() {
				this.isShowAdd = true;
			},
			close() {
				this.isShowAdd = false;
			}
		},
		onLoad() {
			uni.$on('success-add-service', () => {
				this.isShowAdd = false;
			})
		}
	}
</script>

<style lang="scss">
	page {
		padding-bottom: 40rpx;
		.service {
			margin: 80rpx 60rpx;
			.service-add {
				text-align: center;
				margin: 30rpx 0;
				& > image {
					width: 80rpx;
				}
			}
			.service-title {
				display: flex;
				justify-content: space-between;
				align-items: center;
				margin-bottom: 80rpx;
				padding: 0 20rpx;
				position: relative;

				.service-title_text {
					font-size: 40rpx;
				}

				.service-title-img {
					image {
						position: absolute;
						top: -70rpx;
						right: 0;
						width: 200rpx;
					}
				}
			}

			.service-head {
				display: flex;
				justify-content: center;
				margin-bottom: 50rpx;

				.service-head_item {
					flex: 1;
					text-align: center;
					color: #fff;
					padding: 10rpx 20rpx;
					color: #919191;
					border-radius: 12px;
					margin: 0 10rpx;
				}

				.activeService {
					color: #6ea0f0;
					background-color: #eaebf7;
				}
			}
		}
	}
</style>
