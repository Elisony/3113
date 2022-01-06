<template>
	<view class="nav-box">
		<navigator url="/pages/main/main" hover-class="navigator-hover" class="nav-item">
			<view class="head-img">
				<image :src="userData.pic" mode=""></image>
			</view>
			<span class="nav-name">
				{{ userData.user_name }}
			</span>
		</navigator>
		<view class="nav-center" @click="editStatus">
			<view class="nav-status">
				<view class="status-img">
					<image src="/static/images/shougong.png" mode=""></image>
				</view>
				<span class="status-title">
					{{title?title:'收工'}}
				</span>
			</view>
		</view>
		<navigator url="/pages/news/news" hover-class="navigator-hover" class="nav-right">
			<view class="message-sup">
				{{UnreadMessage}}
			</view>
			<view class="message-box">
				<image src="/static/images/message.png" mode=""></image>
			</view>
		</navigator>
	</view>
</template>

<script>
	export default {
		props: ['userData', 'UnreadMessage', 'title'],
		data() {
			return {

			};
		},
		onLoad() {

		},
		methods: {
			editStatus() {
				if (this.title == '开始接单') {
					uni.request({
						url: "http://test.qd-happy.com/app_service",
						method: "POST",
						header: {
							'Content-Type': "multipart/form-data",
						},
						data: {
							interface: "users_upWorkingStatus",
							data: {
								user_id: this.userData.user_id,
								up_status: 1
							}
						},
						success: (res) => {
							console.log(res.data)
							uni.navigateTo({
								url: '/pages/orderTaking/orderTaking'
							})
						}

					})
				} else {
					//更改骑手状态
					uni.request({
						url: "http://test.qd-happy.com/app_service",
						method: "POST",
						header: {
							'Content-Type': "multipart/form-data",
						},
						data: {
							interface: "users_upWorkingStatus",
							data: {
								user_id: this.userData.user_id,
								up_status: 2
							}
						},
						success: (res) => {
							console.log(res.data)
							if (res.data.error_code === '000000') {
								uni.navigateTo({
									url: '/pages/finishWork/finishWork'
								});
							} else {
								uni.showModal({
									title: '温馨提示',
									content: res.data.code_message,
									success: function(res) {

									}
								});
							}
						}
					})
				}
			}
		}
	}
</script>

<style>
	.nav-box {
		width: 100%;
		height: 84px;
		background-color: #308bd1;
		display: flex;
		flex-direction: row;
		justify-content: space-around;
	}

	.nav-item {
		width: 22%;
		height: 44px;
		margin-top: 30px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: flex-start;
	}

	.nav-center {
		width: 30%;
		height: 44px;
		margin-top: 30px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}

	.head-img {
		width: 40px;
		height: 40px;
		border-radius: 50%;
		overflow: hidden;
	}

	.head-img>image {
		width: 100%;
		height: 100%;
	}

	.nav-name {
		font-size: 12px;
		color: #fff;
		padding-left: 10px;
	}

	.nav-status {
		width: 94px;
		height: 29px;
		border: 1px solid #fff;
		border-radius: 30px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}

	.status-img {
		width: 19px;
		height: 17px;
	}

	.status-img>image {
		width: 100%;
		height: 100%;
	}

	.status-title {
		font-size: 15px;
		color: #fff;
		padding-left: 10px;
	}

	.message-box {
		width: 17px;
		height: 21px;
		z-index: 19;
	}

	.message-box>image {
		width: 100%;
		height: 100%;
	}

	.nav-right {
		width: 22%;
		height: 44px;
		margin-top: 30px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: flex-end;
		position: relative;
	}

	.message-sup {
		width: 15px;
		height: 15px;
		background-color: red;
		border-radius: 50%;
		font-size: 10px;
		line-height: 15px;
		text-align: center;
		color: #fff;
		position: absolute;
		top: 6px;
		right: -9px;
		z-index: 20;
	}
</style>
