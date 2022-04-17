<template>
	<view class="add-object-container">
		<view class="add-object">
			<image @click.stop="close" src="https://play-in-gdufs.oss-cn-beijing.aliyuncs.com/shanchu.png"
				mode="widthFix"></image>
			<view class="add-object_img" @click="uploadImg">
				<image :src="isUpload ? img : 'https://play-in-gdufs.oss-cn-beijing.aliyuncs.com/tianjiatupian.png'">
				</image>
				<text>{{ isUpload ? 1 : 0}} / 1</text>
			</view>
			<view class="add-object_content">
				<textarea v-model="content" placeholder="请输入您想发布的内容" />
			</view>
			<view class="add-object_picker-type">
				物品类别：
				<picker mode="selector" :range="type" :value="typeIndex" @change="handleChange('type',$event)">
					<text>{{type[typeIndex]}}</text>
					<image src="https://play-in-gdufs.oss-cn-beijing.aliyuncs.com/选择按钮.png" mode="widthFix"></image>
				</picker>
			</view>
			<view class="add-object_picker-category">
				分类：
				<picker mode="selector" :range="category" :value="categoryIndex"
					@change="handleChange('category',$event)">
					<text>{{category[categoryIndex]}}</text>
					<image src="https://play-in-gdufs.oss-cn-beijing.aliyuncs.com/选择按钮.png" mode="widthFix"></image>
				</picker>
			</view>
			<view class="add-object_btn">
				<button plain @click="publish">发布</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "add-service-object",
		data() {
			return {
				id: '',
				img: '',
				isUpload: false,
				content: '',
				path: '',
				typeIndex: 0,
				type: ['电子设备', '校卡', '雨具', '衣物', '其他'],
				categoryIndex: 0,
				category: ['找失主', '找物品']
			};
		},
		methods: {
			handleChange(string, $event) {
				(string === 'type') && (this.typeIndex = $event.detail.value);
				(string === 'category') && (this.categoryIndex = $event.detail.value);
			},
			uploadImg() {
				uni.chooseImage({
					count: 1,
					success: res => {
						uni.showLoading({
							title: '上传中'
						})
						this.img = res.tempFilePaths[0];
						this.isUpload = true;
						uni.uploadFile({
							url: 'https://gdufs.yansp.xyz/common/upload',
							filePath: this.img,
							name: 'image',
							success: uploadFileRes => {
								uni.hideLoading();
								uni.showToast({
									icon: 'success',
									title: '上传成功!'
								})
								this.path = JSON.parse(uploadFileRes.data).data.path;
							}
						})
					}
				})
			},
			publish() {
				this.$request({
					url: '/lost/add',
					data: {
						userId: this.id,
						content: this.content,
						path: this.path,
						itemType: +this.typeIndex + 1,
						searchType: +this.categoryIndex + 1,
					}
				}).then(res => {
					if (res.data.code === 200) {
						uni.$emit('success-add-service');
						this.content = '';
						this.img = '';
						this.isUpload = false;
						uni.showToast({
							icon: 'success',
							title: '发布成功！'
						})
					} else {
						uni.showToast({
							icon: 'error',
							title: '操作失败！'
						})
					}
				})
			},
			close() {
				this.content = '';
				this.img = '';
				this.isUpload = false;
				this.$emit('close');
			}
		},
		created() {
			uni.getStorage({
				key: 'id',
				success: res => {
					this.id = res.data;
				}
			})
		}
	}
</script>

<style lang="scss">
	.add-object-container {
		position: fixed;
		background-color: rgba(0, 0, 0, 0.6);
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
		z-index: 100000;

		.add-object {
			width: 85%;
			position: absolute;
			top: 50%;
			left: 50%;
			transform: translate(-50%, -50%);
			z-index: 200000;
			background-color: #fff;
			border-radius: 20px;
			padding: 60rpx;
			box-sizing: border-box;

			.add-object_picker-type {
				text-align: left;
				display: flex;
				align-items: center;
				margin: 20rpx 0;
				padding-left: 20rpx;
				font-size: 28rpx;

				picker {
					border: 1px solid #808080;
					border-radius: 8px;
					padding: 5rpx 20rpx;

					image {
						width: 18rpx;
					}

					text {
						margin-right: 5rpx;
						color: #808080;
					}
				}
			}

			.add-object_picker-category {
				text-align: left;
				display: flex;
				align-items: center;
				margin: 20rpx 0;
				padding-left: 20rpx;
				font-size: 28rpx;

				picker {
					border: 1px solid #808080;
					border-radius: 8px;
					padding: 5rpx 20rpx;

					image {
						width: 18rpx;
					}

					text {
						margin-right: 5rpx;
						color: #808080;
					}
				}
			}

			&>image {
				width: 50rpx;
				position: absolute;
				top: 10rpx;
				right: 15rpx;
			}

			.add-object_img {
				display: flex;
				justify-content: space-between;
				align-items: flex-end;
				width: 92%;
				margin: 0 auto;
				margin-top: 20rpx;

				text {
					color: #FFa365;
				}

				image {
					width: 50rpx;
					height: 50rpx;
				}
			}

			.add-object_content {
				display: flex;

				textarea {
					background-color: rgba(255, 210, 180, 0.5);
					border-radius: 12px;
					padding-left: 30rpx;
					padding-top: 20rpx;
					margin-top: 40rpx;
					text-align: left;
				}
			}

			.add-object_btn {
				margin-top: 50rpx;
				display: flex;

				button {
					height: 60rpx;
					line-height: 60rpx;
					border: none;
					background-color: #FFa365;
					color: #fff;
					border-radius: 20px;
					padding: 0 60rpx;
					font-size: 32rpx;
				}
			}
		}
	}
</style>
