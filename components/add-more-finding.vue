<template>
	<view class="add-more-finding-container">
		<view class="add-more-finding">
			<image class="add-more-finding-close" @click="close" src="https://play-in-gdufs.oss-cn-beijing.aliyuncs.com/shanchu.png" mode="widthFix"></image>
			<view class="add-more-finding_img" @click="uploadImg">
				<image :src="isUpload ? img : 'https://play-in-gdufs.oss-cn-beijing.aliyuncs.com/tianjiatupian.png'">
				</image>
				<text>{{ isUpload ? 1 : 0}} / 1</text>
			</view>
			<view class="add-more-finding_content">
				<textarea v-model="content" placeholder="请输入您想分享的内容" />
			</view>
			<view class="add-more-finding_btn">
				<button plain @click="publish">发布</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "add-more-finding",
		data() {
			return {
				id: '',
				img: '', // 上传的图片的临时链接
				path: '', // 上传到服务器的图片链接
				isUpload: false,
				content: ''
			};
		},
		methods: {
			uploadImg() { // 上传图片
				uni.chooseImage({
					count: 1,
					success: res => {
						uni.showLoading({
							title: '上传中'
						})
						this.img = res.tempFilePaths[0];
						this.isUpload = true;
						console.log('上传还没成功！！！！',res);
						uni.uploadFile({
							url: 'https://gdufs.yansp.xyz/common/upload',
							filePath: this.img,
							name: 'image',
							header: {
								'content-type': 'multipart/form-data'
							},
							success: uploadFileRes => {
								console.log('上传成功啦！！！！');
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
					url: '/common/share',
					data: {
						content: this.content,
						path: this.path,
						userId: this.id,
					}
				}).then(res => {
					if(res.data.code === 200) {
						uni.$emit('success-add-finding');
						this.content = '';
						this.img = '';
						this.isUpload = false;
						uni.showToast({
							icon: 'success',
							title: '发布成功！'
						})
					}else {
						uni.showToast({
							icon: 'error',
							title: '操作失败！'
						})
					}
				},err => {
					uni.showToast({
						icon: 'error',
						title: '操作失败！'
					})
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
	.add-more-finding-container {
		position: fixed;
		background-color: rgba(0, 0, 0, 0.6);
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
		z-index: 100000;

		.add-more-finding {
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
			
			.add-more-finding-close {
				width: 50rpx;
				position: absolute;
				top: 10rpx;
				right: 15rpx;
			}
			.add-more-finding_img {
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

			.add-more-finding_content {
				display: flex;
				textarea {
					background-color: rgba(255, 210, 180, 0.5);
					border-radius: 12px;
					padding-left: 30rpx;
					padding-top: 20rpx;
					margin-top: 40rpx;
				}
			}

			.add-more-finding_btn {
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
