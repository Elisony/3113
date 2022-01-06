<template>
	<view class="content">
		<navs :userData="userData"></navs>
		<view class="task-list" v-show="listShow">
			<view v-for="(item, index) in orderProgress" :key="index" class="order-list">
				<view class="order-list" v-if="item.status == 0">
					<navigator :url="'/pages/orderReceived/orderReceived?id='+item.order_id"
						hover-class="navigator-hover">
						<view class="list-top">
							<view class="top-left">
								{{ item.sum_account }}
							</view>
							<view class="top-right">
								订单编号：{{ item.sn }}
							</view>
						</view>
						<purchasingAgent :item="item"></purchasingAgent>
					</navigator>
					<view class="order-title">
						标题：{{ item.title }}
					</view>
					<view class="order-title">
						备注：{{ item.note }}
					</view>
					<view class="button-box">
						<button class="button-left" @click="refuseOrder(item.order_id)">拒接</button>
						<button class="button-right" @click="receivingOrder(item.order_id)">接单</button>
					</view>
				</view>
				<view class="order-list" v-else-if="item.status == 1">
					<navigator :url="'/pages/orderReceived/orderReceived?id='+item.order_id"
						hover-class="navigator-hover">
						<view class="list-top">
							<view class="top-left">
								{{ item.sum_account }}
							</view>
							<view class="top-right">
								订单编号：{{ item.sn }}
							</view>
						</view>
						<purchasingAgent :item="item"></purchasingAgent>
					</navigator>
					<view class="order-title">
						标题：{{ item.title }}
					</view>
					<view class="order-title">
						备注：{{ item.note }}
					</view>
					<view class="button-box">
						<button class="button-left" @click="cancelOrder(item.order_id)">取消</button>
						<button class="button-ongoing">进行中...</button>
					</view>
				</view>
			</view>
		</view>

		<view class="list-none-box" v-show="listNone">
			<view class="swiper-box">
				<swiper class="swiper" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval"
					:duration="duration" indicator-color="#fff" indicator-active-color="#308bd1">
					<swiper-item class="swiper-item">
						<image src="/static/images/banner3.jpg" mode=""></image>
					</swiper-item>
					<swiper-item class="swiper-item">
						<image src="/static/images/banner3.jpg" mode=""></image>
					</swiper-item>
				</swiper>
			</view>
			<view class="none-info">
				<view class="none-info-img">
					<image src="/static/images/list-none.png" mode=""></image>
				</view>
				<span class="none-info-title">暂无订单</span>
			</view>
		</view>
		<!-- <alert :QHeight="QHeight" :alertData="alertData"></alert> -->
		<tab></tab>
	</view>
</template>

<script>
	import navs from '../../components/nav.vue'
	import tab from '../../components/tab.vue'
	import purchasingAgent from '../../components/purchasingAgent.vue'
	import retail from '../../components/retail.vue'
	import alert from '../../components/alert.vue'

	export default {
		components: {
			navs,
			tab,
			purchasingAgent,
			retail,
			alert,
		},
		data() {
			return {
				listNone: false,
				listShow: true,
				indicatorDots: true,
				autoplay: true,
				interval: 2000,
				duration: 500,
				QHeight: 0,
				alertData: {
					title: '取消订单成功',
					content: '您当天仅剩2次取消机会',
					button: '确定'
				},
				userData: {},
				orderProgress: []
			}
		},
		onLoad: function(option) {
			let then = this

			//获取跳转参数
			let userinfo = uni.getStorageSync('userinfo')
			then.userData = userinfo
			// then.userData = option
			// console.log(then.userData)
			//获取屏幕高度
			uni.getSystemInfo({
				success: (res) => {
					let height = res.windowHeight - uni.upx2px(0)
					// console.log(height)
					then.QHeight = height
				}
			})

			//实时上报位置
			then.getLocal()

			//获取骑手进行中订单
			then.getOrderProgress()

			//获取banner图片列表
			then.getBannerImage()
		},

		methods: {
			//实时上报骑手位置
			getLocal() {
				// var time =setInterval(function() {
				let then = this
				if (uni.getStorageSync('userinfo')) {
					uni.getLocation({
						type: 'gcj02 ',
						success: function(res) {
							console.log(res, '哈哈哈')
							uni.setStorageSync('userLocation', JSON.stringify(res));
							// 这里请求是把数据传给后台
							uni.request({
								url: "http://test.qd-happy.com/app_service",
								method: "POST",
								header: {
									'Content-Type': "multipart/form-data",
								},
								data: {
									interface: "users_upCoordinates",
									data: {
										user_id: then.userData.user_id,
										longitude: res.longitude, //经度
										latitude: res.latitude //纬度
									}
								},
								success: (res) => {

								}
							})
						},
						fail: function() {
							uni.showToast({
								title: '获取位置失败',
								icon: 'none'
							});
						}
					});
				}
				// }, 10000);  
			},

			//获取骑手跑腿中订单
			getOrderProgress() {
				let then = this
				if (uni.getStorageSync('userinfo')) {
					console.log(this.userData.user_id, 'id啊')
					uni.request({
						url: "http://test.qd-happy.com/app_service",
						method: "POST",
						header: {
							'Content-Type': "multipart/form-data",
						},
						data: {
							interface: "order_getOngoingOrderList",
							data: {
								user_id: then.userData.user_id,
							}
						},
						success: (res) => {
							console.log(res.data.data.list, '？？？')
							then.orderProgress = res.data.data.list
							if (res.data.data.list.length == 0) {
								then.listNone = true
								then.listShow = false
							}
						},
						fail: function() {
							uni.showToast({
								title: '订单获取失败',
								icon: 'none'
							});
						}
					})
				}
			},

			//接单
			receivingOrder(order_id) {
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
							order_id: order_id,
							status: 1
						}
					},
					success: (res) => {
						uni.showToast({
							title: res.code_message,
							icon: 'none'
						});
						uni.navigateTo({
							url: '/pages/orderTaking/orderTaking'
						});
					},
					fail: function() {
						uni.showToast({
							title: '接单失败',
							icon: 'none'
						});
					}
				})
			},

			//拒绝订单
			refuseOrder(order_id) {
				uni.request({
					url: "http://test.qd-happy.com/app_service",
					method: "POST",
					header: {
						'Content-Type': "multipart/form-data",
					},
					data: {
						interface: "order_refuseOrder",
						data: {
							user_id: this.userData.user_id,
							order_id: order_id
						}
					},
					success: (res) => {
						uni.showToast({
							title: '订单拒绝成功',
							icon: 'none'
						});
					},
					fail: function() {
						uni.showToast({
							title: '拒绝订单失败',
							icon: 'none'
						});
					}
				})
			},

			//取消订单
			cancelOrder(order_id) {
				uni.navigateTo({
					url: '/pages/orderCancel/orderCancel?id=' + order_id
				});
			},

			//获取banner图片列表
			getBannerImage() {
				uni.request({
					url: "http://test.qd-happy.com/app_service",
					method: "POST",
					header: {
						'Content-Type': "multipart/form-data",
					},
					data: {
						interface: "banner_getBannerImage",
						data: {

						}
					},
					success: (res) => {
						console.log(res.data)
					},
					fail: function() {
						uni.showToast({
							title: '获取banner失败',
							icon: 'none'
						});
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

	.none-info-title {
		font-size: 15px;
		color: #9b9b9b;
		line-height: 50px;
	}

	.none-info-img {
		width: 137px;
		height: 90px;
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

	.swiper-box {
		width: 100%;
		height: 100px;
	}

	.swiper {
		width: 100%;
		height: 100px;
	}

	.swiper-item {
		width: 100%;
		height: 100px;
	}

	.swiper-item>image {
		width: 100%;
		height: 100%;
	}

	.list-none-box {
		width: 100%;
		height: 100px;
		z-index: 19;
	}

	.task-list {
		width: 100%;
		height: 100%;
		z-index: 19;
		margin-bottom: 50px;
	}

	.content {
		width: 100%;
		height: 100%;
		position: relative;
	}

	.order-list {
		width: 100%;
		height: 100%;
		box-shadow: 0px 5px 10px 0px #ccc;
		margin-bottom: 10px;
		padding-bottom: 10px;
	}

	.list-top {
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		border-bottom: 1px solid #eee;
	}

	.top-left {
		width: 36%;
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
		width: 56%;
		height: 47px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: flex-end;
		font-size: 15px;
		color: #000033;
	}

	.order-title {
		width: 90%;
		height: 100%;
		font-size: 15px;
		color: #d13043;
		font-weight: 600;
		margin: 0 auto;
		line-height: 40px;
	}

	.button-box {
		width: 100%;
		height: 50px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}

	.button-left {
		width: 22%;
		background-color: #d13043;
		color: #fff;
		font-size: 18px;
		font-weight: 600;
	}

	.button-right {
		width: 67%;
		background-color: #308bd1;
		color: #fff;
		font-size: 18px;
		font-weight: 600;
	}

	.button-ongoing {
		width: 67%;
		background-color: #52d130;
		color: #fff;
		font-size: 18px;
		font-weight: 600;
	}
</style>
