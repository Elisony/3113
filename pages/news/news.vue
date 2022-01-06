<template>
	<view class="content">
		<view class="top-back" @tap="navigateBack">
			<view class="top-left">
				<view class="back-box">
					<image src="/static/images/back.png" mode=""></image>
				</view>
			</view>
			<view class="top-right">
				消息中心
			</view>
		</view>
		<view v-for="(item, index) in csListArrl" :key="index" :data-index="index" class="news-item"
			@touchstart="drawStart" @touchmove="drawMove" @touchend="drawEnd" :style="'right:'+item.right+'px'">
			<view class="news-item-info">
				<view class="news-item-line1">
					<view class="item-left">
						<span class="left-spot">.</span>
						<span class="left-title">{{item.title}}</span>
					</view>
				</view>
				<view class="news-item-line2">
					{{item.content}}
				</view>
			</view>
			<view class="remove" @click="delData(item)">
				删除
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				csListArrl: [],
				//左滑默认宽度
				delBtnWidth: 80,
				page:1
			}
		},
		onLoad() {
			this.getInfo(this.page)
		},
		methods: {
			getInfo() {
				let userData = uni.getStorageSync('userinfo')
				uni.request({
					url: "http://test.qd-happy.com/app_service",
					method: "POST",
					header: {
						'Content-Type': "multipart/form-data",
					},
					data: {
						interface: "users_getMsgList",
						data: {
							user_id: userData.user_id,
							page: this.page
						}
					},
					success: (res) => {
						this.csListArrl=res.data.data.list
						console.log(res)
					}
				})
			},
			//开始触摸滑动
			drawStart(e) {
				console.log("开始触发");
				var touch = e.touches[0];
				this.startX = touch.clientX;
			},
			//触摸滑动
			drawMove(e) {
				console.log("滑动");

				for (var index in this.csListArrl) {
					this.$set(this.csListArrl[index], 'right', 0);
				}
				var touch = e.touches[0];
				var item = this.csListArrl[e.currentTarget.dataset.index];
				var disX = this.startX - touch.clientX;

				if (disX >= 20) {
					if (disX > this.delBtnWidth) {
						disX = this.delBtnWidth;
					}
					this.$set(this.csListArrl[e.currentTarget.dataset.index], 'right', disX);
				} else {
					this.$set(this.csListArrl[e.currentTarget.dataset.index], 'right', 0);
				}
			},
			//触摸滑动结束
			drawEnd(e) {
				console.log("滑动结束");
				var item = this.csListArrl[e.currentTarget.dataset.index];
				if (item.right >= this.delBtnWidth / 2) {
					this.$set(this.csListArrl[e.currentTarget.dataset.index], 'right', this.delBtnWidth);
				} else {
					this.$set(this.csListArrl[e.currentTarget.dataset.index], 'right', 0);
				}
			},
			//删除方法
			delData(item) {
				console.log("删除")
				uni.showModal({
					title: '提示',
					content: "确认注销该人员？",
					success: function(res) {
						if (res.confirm) {
							console.log('用户点击确定');
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			navigateBack() {
				uni.navigateBack()
			}
		}
	}
</script>

<style>
	body {
		background-color: #fafafa;
	}

	.content {
		width: 100%;
		height: 100%;
	}

	.top-back {
		width: 100%;
		height: 44px;
		padding-top: 20px;
		display: flex;
		flex-direction: row;
		line-height: 50px;
		background-color: #308bd1;
	}

	.top-left {
		width: 42%;
		height: 30px;
	}

	.back-box {
		width: 7px;
		height: 12px;
		margin-left: 10px;
	}

	.back-box>image {
		width: 100%;
		height: 100%;
	}

	.top-right {
		width: 58%;
		height: 30px;
		color: #fff;
		font-size: 16px;
	}

	.news-item {
		width: 100%;
		height: 90px;
		background-color: #fff;
		border-top: 1px solid #eee;
		border-bottom: 1px solid #eee;
		margin-top: 10px;
		position: relative;
	}

	.news-item-info {
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.news-item-line1 {
		width: 92%;
		height: 40px;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: flex-start;
		margin-top: 10px;
	}

	.item-left {
		width: 100%;
		height: 30px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: flex-start;
	}

	.left-spot {
		font-size: 30px;
		color: red;
		padding-bottom: 20px;
	}

	.left-title {
		font-size: 15px;
		color: #333333;
		padding-left: 10px;
	}

	.news-item-line2 {
		width: 90%;
		height: 30px;
		font-size: 12px;
		color: #999999;
	}

	.remove {
		width: 80px;
		height: 100%;
		background-color: red;
		color: white;
		position: absolute;
		top: 0;
		right: -80px;
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 16px;
	}
</style>
