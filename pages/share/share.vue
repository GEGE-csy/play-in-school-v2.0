<template>
	<view class="share">
		<view class="share-bar">
			<view class="share-add-btn">
				<image v-show="currentIndex === 1" @click="addMore" src="https://play-in-gdufs.oss-cn-beijing.aliyuncs.com/tianjia.png" mode="widthFix"></image>
				<add-more-finding v-show="isShow" @close="close"></add-more-finding>
			</view>
			<view>
				<view class="share-bar-item" @click="switchBar(index)" v-for="(item,index) in choose" :key="index"
					:style="{boxShadow: currentIndex === index ? '0 5px 6px 0 rgba(0, 0, 0, 0.16)': ''}">
					{{ item }}
				</view>
			</view>
		</view>
		<view v-if="currentIndex === 0">
			<scene></scene>
		</view>
		<view v-else>
			<more-finding></more-finding>
		</view>
	</view>
</template>

<script>
	import scene from '../../components/scene.vue';
	import moreFinding from '../../components/more-finding.vue';
	import addMoreFinding from '../../components/add-more-finding.vue';
	export default { // 分享圈
		data() {
			return {
				choose: ['附近景点', '更多发现'],
				currentIndex: 0,
				isShow: false,
			}
		},
		components: {
			scene,
			moreFinding,
			addMoreFinding
		},
		methods: {
			switchBar(index) {
				this.currentIndex = index;
			},
			addMore() {
				this.isShow = true;
			},
			close() {
				this.isShow = false;
			}
		},
		onLoad() {
			uni.$on('success-add-finding', () => {
				this.isShow = false;
			})
		},
		
	}
</script>

<style lang="scss">
	.share {
		.share-bar {
			display: flex;
			justify-content: space-between;
			width: 85%;
			margin: 0 auto;
			align-items: center;
			.share-add-btn {
				display: flex;
				align-items: center;
				image {
					width: 50rpx;
				}
			}

			view:nth-child(2) {
				display: flex;

				.share-bar-item {
					padding: 10rpx 30rpx;
					border-radius: 20px;
					color: #fff;
					font-size: 28rpx;

					&:nth-child(1) {
						background-color: #Fbc445;
						margin-right: 20rpx;
					}

					&:nth-child(2) {
						background-color: #FFa365;
					}
				}
			}

			
		}
	}
</style>
