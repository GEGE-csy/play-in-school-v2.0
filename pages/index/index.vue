<template>
	<view class="welcome">
		<view class="welcome-title">玩<br>转校园</view>
		<view class="welcome-content">这里有很全面的校园消息..</view>
		<view class="welcome-btn" @click="getUserInfo">点击开始这段旅程</view>
		<image class="welcome-img" :src="welcomeImg" mode="widthFix"></image>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				name: '',
				profile: '',
				welcomeImg: 'https://play-in-gdufs.oss-cn-beijing.aliyuncs.com/temp/welcome%E8%83%8C%E6%99%AF2.png'
			}
		},
		methods: {
			getUserInfo() {
				uni.getUserProfile({
					desc: '登录',
					lang: 'zh_CN',
					success: res => {
						uni.showLoading({
							title: '加载中'
						})
						uni.setStorage({
							key: 'userInfo',
							data: res.userInfo
						})
						uni.login({
							provider: 'login',
							success: res => {
								console.log(res.code);
								this.$request({
									url: '/common/login',
									method: 'POST',
									header: {
										'content-type': 'application/x-www-form-urlencoded'
									},
									data: {
										code: res.code
									}
								}).then(res => {
									console.log(res.data);
									const {id} = res.data.data;
									uni.setStorage({
										key: 'id',
										data: id,
										success: () => {
											uni.hideLoading();
											uni.showToast({
												icon: 'success',
												title: '登录成功！',
												success: () => {
													uni.navigateTo({
														url: '/pages/home/home'
													})
												}
											})
										}
									})
								})
							}
						})
						
					},
				});	
				
			
				// const [err, res] = await uni.login({ // 拿到res.code
				// 	provider: 'login',
				// });
				// const data = await this.$request({	// 拿到请求返回的data
				// 	url: '/admin/login',
				// 	data: {
				// 		number: e.detail.value.number,
				// 		password: e.detail.value.password,
				// 		code: res.code,
				// 	},
				// 	loadingText: '正在登录'
				// });
				// uni.setStorage({ // 将数据存在本地
				// 	key: 'userinfo',
				// 	data: data.data,
				// 	success: () => {
				// 		uni.navigateTo({ // 跳转教师主页，这里因为tabbar是自制的，所以不用switchTab而是navigateTo
				// 			url: '/pages/teacher/teacher'
				// 		})
				// 	}
				// })
			}
		},
	}
</script>

<style lang="scss">
	page {
		background-color: #CED6EF;
		height: 100%;
		display: flex;
		justify-content: center;
		align-items: center;

		.welcome {
			width: 70%;

			.welcome-title {
				color: #fff;
				font-size: 60rpx;
				margin-bottom: 20rpx;
				font-weight: bolder;
			}

			.welcome-content {
				color: #fff;
				margin-bottom: 40rpx;
			}

			.welcome-btn {
				display: inline-block;
				border-radius: 20rpx;
				background-color: #fff;
				color: #81a4ef;
				padding: 24rpx 40rpx;
			}

			.welcome-img {
				margin-top: 60rpx;
				width: 500rpx;
			}
		}
	}
</style>
