<template>
	<view class="book-list">
		<view class="book-item" v-for="(item,index) in bookList" :key="item.id">
			<view class="book-item-info">
				<image :src="item.cover" mode="widthFix"></image>
				<view class="book-detail">
					<view class="book-name">
						{{item.name}}
					</view>
				</view>
			</view>
			<view class="book-item-op">
<!-- 				<view @click="downloadFile(index,idList.includes(item.id),item.id)" class="book-item-download">
					{{idList.includes(item.id) ? '打开': '下载'}}
				</view> -->
				<view @click="previewFile(index)" class="book-item-review">
					预览
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "book-list",
		props: ['bookList'],
		methods: { 
			previewFile(index) {
				uni.showLoading({
					title: '正在加载...'
				})
				this.$request({
					url: '/book/preview',
					method: 'POST',
					data: {
						page: 1,
						path: this.bookList[index].path
					},
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					responseType: 'arraybuffer' // 流
				}).then(res => {
					const fileManager = uni.getFileSystemManager(); // 获取全局的文件管理器
					const filePath = `${wx.env.USER_DATA_PATH}/${this.bookList[index].name}.pdf`; // 文件存储到本地的路径
					fileManager.writeFile({
						data: res.data,
						filePath: filePath,
						encoding: 'binary',
						success: res => {
							uni.openDocument({ // 直接打开
								filePath: filePath,
								fileType: 'pdf',
								showMenu: true,
								success: res => {
									uni.hideLoading();
								}
							})
						}
					})
				})
			},
		},
	}
</script>

<style lang="scss">
	.book-list {
		padding: 15px 20px;

		.book-item {
			font-size: 14px;
			padding: 15px;
			background-color: #fff;
			border-radius: 12px;
			display: flex;
			justify-content: space-between;
			margin-top: 15px;

			&:first-child {
				margin-top: 0;
			}

			.book-item-info {
				display: flex;
				align-items: center;

				image {
					width: 100rpx;
					margin-right: 34rpx;
				}

				.book-detail {
					display: flex;
					flex-direction: column;
					justify-content: space-around;
					height: 100%;

					.book-name {
						width: 240rpx;
						font-size: 32rpx;
					}
				}
			}

			.book-item-op {
				display: flex;
				align-items: center;

				.book-item-review {
					margin-right: 10px;
					padding: 5px 10px;
					border-radius: 8px;
					background-color: #FAE7E0;
					text-align: center;
					white-space: nowrap;
				}

				// .book-item-review {
				// 	padding: 5px 10px;
				// 	border-radius: 8px;
				// 	border: 1px solid #FAE7E0;
				// 	text-align: center;
				// 	white-space: nowrap;
				// }
			}
		}
	}
</style>
