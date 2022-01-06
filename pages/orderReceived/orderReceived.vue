<template>
	<view class="content">
		<navs :userData="userData"></navs>
		<view class="order-list">
			<view class="list-top">
				<view class="top-more">
					<view class="more-img">
						<image src="/static/images/number.png" mode=""></image>
					</view>
					<span class="more-number">
						{{ orderInfo.customer_sn }}
					</span>
				</view>
				<view class="top-left">
					￥{{ orderInfo.account }}
				</view>
				<view class="top-right">

				</view>
			</view>
			<dispatchingInfo :item="orderInfo"></dispatchingInfo>
			<view class="order-title">
				{{ orderInfo.title }}
			</view>
			<view class="order-title">
				{{ orderInfo.note }}
			</view>
			<view class="button-box">
				<view class="button-left">
					<!-- <button class="button-buy">帮买</button>
					<button class="button-send">帮取送</button> -->
				</view>
				<view class="button-right">
					<button class="button-temporary" @tap="navigatTo">临时+</button>
				</view>
			</view>
		</view>
		<view class="order-info">
			<navigator v-for="(raw, key) in orderInfo.info_list"
				:url="'/pages/details/details?info_id='+raw.info_id+'&order_id='+orderInfo.order_id"
				hover-class="navigator-hover" class="info-box" :key="key">
				<view class="info-left">
					<view class="info-left-title2" v-if="raw.status_str == '已完成'">{{ raw.status_str }}</view>
					<view class="info-left-title" v-else>已取 已买</view>
					<view class="info-left-img">
						<image :src="raw.pic" mode=""></image>
					</view>
				</view>
				<view class="info-center">
					<span class="info-center-font1">{{ raw.type_names }}</span>
					<span class="info-center-font2">{{ raw.content }}</span>
					<span class="info-center-hr"> </span>
					<span class="info-center-font2">计时：<span v-if="raw.timing === 0">{{ raw.timing }}</span></span>
					<span class="info-center-font2">总样输：{{ raw.num }}</span>
				</view>
				<view class="info-right">
					<button class="info-right-button1" v-if="raw.over_account !== '0.00'">超
						+{{ raw.over_account }}</button>
					<button class="info-right-button2"
						v-if="raw.info_account !== '0.00'">￥{{ raw.info_account }}</button>
				</view>
			</navigator>
			<!-- <navigator url="/pages/details/details"  hover-class="navigator-hover" class="info-box">
				<view class="info-left">
					<view class="info-left-title">已取 已买</view>
					<view class="info-left-img">
						<image src="/static/images/yaopin.jpg" mode=""></image>
					</view>
				</view>
				<view class="info-center">
					<span class="info-center-font1">药品|阿莫西林胶囊</span>
					<span class="info-center-font2">备注内容</span>
					<span class="info-center-hr"> </span>
					<span class="info-center-font2">计时： 13分钟</span>
					<span class="info-center-font2">总样输： 1</span>
				</view>
				<view class="info-right">
					<button class="info-right-button1">超+50</button>
					<button class="info-right-button2">￥250</button>
				</view>
			</navigator> -->
		</view>
		<tab :title="title" @RefreshList="RefreshList"></tab>
	</view>
</template>

<script>
	import navs from '../../components/nav.vue'
	import tab from '../../components/tab.vue'
	import dispatchingInfo from '../../components/dispatchingInfo.vue'

	export default {
		components: {
			navs,
			tab,
			dispatchingInfo
		},
		data() {
			return {
				userData: {},
				orderInfo: [],
				order_id: 0,
				title: '',
				nextList: ['接单', '到达取货点', '到达送货点', '已交付']
			}
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数

			let then = this
			then.order_id = option.id

			//获取骑手跑腿中详情页面
			then.getOrderInfo()
		},
		methods: {
			//获取骑手跑腿中详情页面
			getOrderInfo() {
				this.userData = uni.getStorageSync('userinfo')
				if (this.userData) {
					uni.request({
						url: "http://test.qd-happy.com/app_service",
						method: "POST",
						header: {
							'Content-Type': "multipart/form-data",
						},
						data: {
							interface: "order_getOrderInfo",
							data: {
								user_id: this.userData.user_id,
								order_id: this.order_id
							}
						},
						success: (res) => {
							this.orderInfo = res.data.data
							this.title = this.nextList[this.orderInfo.next_status - 1]
						}
					})
				}
			},

			//跳转到服务页面
			navigatTo() {
				uni.navigateTo({
					url: '/pages/server/server?order_id=' + this.orderInfo.order_id
				});
			},
			//点击下一步
			RefreshList(val) {
				let nextStatus = this.nextList.indexOf(this.title) + 1
				uni.request({
					url: "http://test.qd-happy.com/app_service",
					method: "POST",
					header: {
						'Content-Type': "multipart/form-data",
					},
					data: {
						interface: "order_upOrderStatus",
						data: {
							user_id: this.userData.user_id,
							order_id: this.order_id,
							status: nextStatus
						}
					},
					success: (res) => {
						this.getOrderInfo()
						uni.showToast({
							title: res.data.code_message
						})
					}
				})
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

	.order-list {
		width: 100%;
		height: 380px;
		box-shadow: 0px 5px 10px 0px #ccc;
		margin-bottom: 10px;
	}

	.list-top {
		width: 100%;
		height: 47px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: space-around;
		border-bottom: 1px solid #eee;
	}

	.top-more {
		width: 30%;
		height: 47px;
		display: flex;
		flex-direction: row;
		align-items: center;
	}

	.more-img {
		width: 27px;
		height: 27px;
	}

	.more-img>image {
		width: 100%;
		height: 100%;
	}

	.more-number {
		font-size: 12px;
		color: #308bd1;
		padding-left: 10px;
	}

	.top-left {
		width: 20%;
		height: 47px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: flex-start;
		font-size: 24px;
		font-weight: 600;
		color: #d13043;
	}

	.top-right {
		width: 30%;
		height: 47px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: flex-end;
		font-size: 12px;
		color: #d13043;
	}

	.item-down-box {
		width: 92%;
		height: 30px;
		display: flex;
		flex-direction: row;
		align-items: center;
	}

	.item-down {
		width: 8px;
		height: 14px;
	}

	.item-down>image {
		width: 100%;
		height: 100%;
	}

	.down-title {
		font-size: 12px;
		color: #ccc;
		padding-left: 10px;
	}

	.left-spot2 {
		font-size: 30px;
		color: red;
		padding-bottom: 20px;
	}

	.order-title {
		width: 90%;
		height: 38px;
		font-size: 15px;
		color: #d13043;
		font-weight: 600;
		margin: 0 auto;
		line-height: 38px;
	}

	.button-box {
		width: 100%;
		height: 40px;
		display: flex;
		flex-direction: row;
		margin-top: 10px;
	}

	.button-left {
		width: 70%;
		height: 40px;
	}

	.button-right {
		width: 30%;
		height: 40px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: flex-end;
	}

	.button-buy {
		width: 30%;
		height: 30px;
		line-height: 30px;
		background-color: #308bd1;
		border-radius: 30px;
		color: #fff;
		font-size: 15px;
		float: left;
		margin-left: 18px;
	}

	.button-send {
		width: 30%;
		height: 30px;
		line-height: 30px;
		background-color: #aaaaaa;
		border-radius: 30px;
		color: #fff;
		font-size: 15px;
		float: left;
		margin-left: 10px;
	}

	.button-temporary {
		width: 70%;
		height: 30px;
		line-height: 30px;
		background-color: #52d130;
		border-radius: 30px;
		color: #fff;
		font-size: 15px;
	}

	.button-ongoing {
		width: 67%;
		background-color: #52d130;
		color: #fff;
		font-size: 18px;
		font-weight: 600;
	}

	.order-info {
		width: 100%;
		height: 100%;
		background-color: #fff;
		display: flex;
		flex-direction: column;
		justify-content: center;
		margin-bottom: 50px;
	}

	.info-box {
		width: 94%;
		height: 125px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}

	.info-left {
		width: 30%;
		height: 100px;
		position: relative;
	}

	.info-center {
		width: 40%;
		height: 100px;
		display: flex;
		flex-direction: column;
	}

	.info-right {
		width: 28%;
		height: 100px;
	}

	.info-left-title {
		position: absolute;
		width: 50px;
		height: 20px;
		background-color: #308bd1;
		font-size: 10px;
		color: #fff;
		text-align: center;
		line-height: 20px;
		border-radius: 0 5px 5px 5px;
		z-index: 30;
	}

	.info-left-title2 {
		position: absolute;
		width: 50px;
		height: 20px;
		background-color: #52d130;
		font-size: 10px;
		color: #fff;
		text-align: center;
		line-height: 20px;
		border-radius: 0 5px 5px 5px;
		z-index: 30;
	}

	.info-left-img {
		width: 106px;
		height: 100px;
	}

	.info-left-img>image {
		width: 100%;
		height: 100%;
	}

	.info-center-font1 {
		font-size: 15px;
		color: #333333;
	}

	.info-center-hr {
		padding-top: 10px;
	}

	.info-center-font2 {
		font-size: 15px;
		color: #999999;
		padding-top: 2px;
	}

	.info-right-button1 {
		width: 100%;
		height: 30px;
		background-color: #d13043;
		font-size: 15px;
		color: #fff;
		text-align: center;
		line-height: 30px;
		border-radius: 30px;
		margin-top: 30px;
		white-space: nowrap
	}

	.info-right-button2 {
		width: 100%;
		height: 30px;
		background-color: #308bd1;
		font-size: 15px;
		color: #fff;
		text-align: center;
		line-height: 30px;
		margin-top: 5px;
		border-radius: 30px;
	}
</style>
