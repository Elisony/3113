<template>
	<view class="content">
		<view class="nav-box">
			<navs :userData="userData" :UnreadMessage='UnreadMessage' :title="title"></navs>
			<!-- 	<view class="nav-item">
				<view class="head-img">
					<image src="/static/images/headimg.jpeg" mode=""></image>
				</view>
				<span class="nav-name">
					小童
				</span>
			</view>
			<view class="nav-right">
				<view class="message-sup">
					15
				</view>
				<view class="message-box">
					<image src="/static/images/message.png" mode=""></image>
				</view>
			</view> -->
		</view>
		<view class="none-info">
			<view class="none-info-img">
				<image src="/static/images/finish-work.png" mode=""></image>
			</view>
			<span class="none-info-title">收工啦...</span>
		</view>
	</view>
</template>

<script>
	import navs from '../../components/nav.vue'
	export default {
		components: {
			navs,
		},
		data() {
			return {
				userData: {},
				UnreadMessage: '',
				title:''
			}
		},
		onLoad() {
			let userinfo = uni.getStorageSync('userinfo')
			this.userData = userinfo
			this.title='开始接单'
			//获取未读消息条数
			uni.request({
				url: "http://test.qd-happy.com/app_service",
				method: "POST",
				header: {
					'Content-Type': "multipart/form-data",
				},
				data: {
					interface: "users_getMsgNum",
					data: {
						user_id: this.userData.user_id,
					}
				},
				success: (res) => {
					this.UnreadMessage = res.data.data.num
				}
			})
		},
		methods: {

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

	.nav-box {
		width: 100%;
		height: 84px;
		background-color: #308bd1;
		display: flex;
		flex-direction: row;
		justify-content: space-around;
	}

	.nav-item {
		width: 50%;
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

	.none-info-title {
		font-size: 15px;
		color: #9b9b9b;
		line-height: 50px;
	}

	.none-info-img {
		width: 84px;
		height: 92px;
	}

	.none-info-img>image {
		width: 100%;
		height: 100%;
	}

	.none-info {
		width: 100%;
		height: 150px;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		margin-top: 130px;
	}
</style>
