<template>
	<view class="network-class">
		<view class="network-class-search">
			<image src="https://play-in-gdufs.oss-cn-beijing.aliyuncs.com/temp/搜索按钮.png" mode="widthFix"></image>
			<input type="text" v-model="content" placeholder="检索" @input="handleSearch"/>
		</view>
		<view class="network-category-bar" :style="{display: !isSearch ? 'flex': 'none'}">
			<view class="network-category-bar-item" @click="switchCategory(index)" v-for="(item,index) in category"
				:key="index"
				:class="{activeItem: currentIndex === index}"
				>
				{{ item }}
			</view>
		</view>
		<view class="class-show-cnt" v-show="isSearch">
			查询到{{ cnt }}条相关信息
		</view>
		<class-list :list="list"></class-list>
	</view>
</template>

<script>
	import classList from '../../components/class-list.vue'
	export default {
		data() {
			return {
				content: '',
				isSearch: false,
				currentIndex: 0,
				category: ['理工类', '文史类', '社科类'],
				list: [],
				cnt: 0,	// 搜索到的视频条目
			}
		},
		components: {
			classList
		},
		methods: {
			switchCategory(index) {
				this.currentIndex = index;
				this.getCourseList();
			},
			getCourseList() {
				this.$request({
					url: '/course/list',
					data: {
						type: this.currentIndex + 1
					}
				}).then(res => {
					this.list = res.data.data;
				})
			},
			handleSearch(e) {
				let content = e.detail.value;
				this.isSearch = content === '' ? false : true;
				if(content === '') {
					this.getCourseList();
				}else {
					this.$request({
						url: '/course/search',
						method: 'POST',
						data: {
							key: content
						},
						header: {
							'content-type': 'application/x-www-form-urlencoded'
						},
					}).then(res => {
						// 找到书
						if (res.data.code === 200) {
							this.list = res.data.data;
							this.cnt = this.list.length;
						} else {	// 找不到书
							this.list = [];
							this.cnt = 0;
						}
					}, reason => {
						uni.showToast({
							title: '查询失败！',
							icon: 'error',
						})
					})
				}
				
			},
		},
		onLoad() {
			this.getCourseList();
		}
	}
</script>

<style lang="scss">
	.network-class {
		padding: 40rpx;

		.network-class-search {
			background-color: #E8eaed;
			border-radius: 12px;
			height: 80rpx;
			display: flex;
			align-items: center;
			padding-left: 20rpx;
			
			image {
				width: 40rpx;
			}

			input {
				color: #030303;
				padding-left: 20rpx;
			}
		}

		.network-category-bar {
			display: flex;
			margin: 40rpx 0;
			margin-top: 60rpx;

			.network-category-bar-item {
				flex: 1;
				background-color: #E8eaed;
				border-radius: 12px;
				padding: 10rpx 0;
				text-align: center;
				margin: 0 10rpx;
			}
			.activeItem {
				background-color:  #Fbc445;
				color: #fff;
				font-weight: bold;
			}
		
		}
		.class-show-cnt {
			text-align: center;
			margin-top: 40rpx;
		}
	}
</style>
