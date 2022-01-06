<template>
	<view class="content">
		<view class="top-back">
			<view class="top-left" @click="navigateBack">
				<view class="back-box">
					<image src="/static/images/back.png" mode=""></image>
				</view>
			</view>
			<view class="top-right">
				取消订单
			</view>
		</view>
		<view class="reason-info">
			<view class="reason-info-top">
				 <p class="reason-top-title">取消原因：{{ cancel_msg }}</p>
				 <p class="reason-top-content">注意：您当天仅剩3次取消机会，完成3次取消之后您本日内不允许进行接单</p>
			</view>
			<view class="reason-info-bottom">
				<p class="reason-bottom-title">若用户同意取消订单，您可以免责取消订单</p>
				<p class="reason-bottom-rule">取消规则</p>
			</view>
		</view>
		<view class="reason-contact">
			<p class="reason-contact-title">{{ userInfo.customer_name }}</p>
			<p class="reason-contact-title">{{ userInfo.customer_phone }}</p>
			<button class="reason-contact-button" @click="phoneCall(userInfo.customer_phone)">拨打电话</button>
		</view>
		<view class="reason-button" @click="chioseCancelStatus(1)">
			协商一致，取消订单
		</view>
		<view class="reason-button" @click="chioseCancelStatus(2)">
			不联系，直接取消
		</view>
		<view class="reason-button" @click="navigateBack">
			放弃取消
		</view>
		<alert :QHeight="QHeight" :alertData="alertData" :show="show"  ref="colose"></alert>
	</view>
</template>

<script>
	import alert from '../../components/alert.vue'
	export default {
		components:{
			alert
		},
		data() {
			return {
				alertData:{
					title: '发送取消订单成功',
					content: '等待客服中心同意',
					button: '确定'
				},
				QHeight: 0,
				show: false,
				cancel_msg: '',
				order_id: 0,
				userInfo: {}
			}
		},
		onLoad: function (option){
			let then = this
			then.order_id = option.order_id
			then.cancel_msg = option.cancel_msg
			//获取屏幕高度
			uni.getSystemInfo({
				success: (res) => {
					let height = res.windowHeight - uni.upx2px(0)
					then.QHeight = height
				}
			})
			
			//获取订单下单客户信息
			then.getMsgNum()
		},
		methods: {
			navigateBack() {
				uni.navigateBack()
			},
			//选择取消选项
			chioseCancelStatus(el){
				let userData = uni.getStorageSync('userinfo')
				uni.request({
					url: "http://test.qd-happy.com/app_service",
					method: "POST",
					header: {
						'Content-Type': "multipart/form-data",
					},
					data: {
						interface: "order_cancelOrder",
						data: {
							user_id: userData.user_id,
							order_id: this.order_id,
							cancel_status: el,
							cancel_msg: this.cancel_msg
						}
					},
					success: (res) => {
						console.log(res)
						uni.showToast({
							title: res.data.code_message,  
							icon: 'none'  
						});  
						if (res.data.error_code == '000000') {
							this.show = true
						}
					}
				})
			},
			
			//获取订单下单客户信息
			getMsgNum(){
				let userData = uni.getStorageSync('userinfo')
				uni.request({
					url: "http://test.qd-happy.com/app_service",
					method: "POST",
					header: {
						'Content-Type': "multipart/form-data",
					},
					data: {
						interface: "order_getOrderCustomer",
						data: {
							user_id: userData.user_id,
							order_id: this.order_id
						}
					},
					success: (res) => {
						this.userInfo = res.data.data
						uni.showToast({
							title: res.data.code_message,  
							icon: 'none'  
						});  
					}
				})
			},
			
			//拨打电话
			phoneCall(phone){
				uni.makePhoneCall({
				    phoneNumber: '10068'
				});
			}
		}
		
		
	}
</script>

<style>
	body{
		background-color: #fafafa;
	}
	.content{
		width: 100%;
		height: 100%;
	}
	.top-back{
		width: 100%;
		height: 44px;
		padding-top: 20px;
		display: flex;
		flex-direction: row;
		line-height: 50px;
		background-color: #308bd1;
	}
	.top-left{
		width: 42%;
		height: 30px;
	}
	.back-box{
		width: 7px;
		height: 12px;
		margin-left: 10px;
	}
	.back-box>image{
		width: 100%;
		height: 100%;
	}
	.top-right{
		width: 58%;
		height: 30px;
		color: #fff;
		font-size: 16px;
	}
	.reason-info{
		width: 100%;
		height: 185px;
		background-color: #fff;
		border-bottom: 1px solid #ccc;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		margin-bottom: 10px;
	}
	.reason-info-top{
		width: 85%;
		height: 105px;
		text-align: center;
		padding: 20px 0;
	}
	.reason-top-title{
		font-size: 18px;
		color: #333;
		line-height: 50px;
		font-weight: 540;
	}
	.reason-top-content{
		font-size: 15px;
		color: #333;
	}
	.reason-info-bottom{
		width: 85%;
		height: 80px;
		text-align: center;
	}
	.reason-bottom-title{
		font-size: 15px;
		color: #999999;
	}
	.reason-bottom-rule{
		font-size: 15px;
		color: #308bd1;
	}
	.reason-contact{
		width: 100%;
		height: 50px;
		background-color: #fff;
		border-bottom: 1px solid #ccc;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: space-around;
	}
	.reason-contact-title{
		width: 30%;
		font-size: 15px;
		color: #333;
		font-weight: 600;
		text-align: center;
	}
	.reason-contact-button{
		width: 25%;
		height: 30px;
		background-color: #308bd1;
		font-size: 15px;
		color: #fff;
		font-weight: 600;
		line-height: 30px;
		
	}
	.reason-button{
		width: 80%;
		height: 45px;
		background-color: #308bd1;
		font-size: 18px;
		color: #fff;
		font-weight: 600;
		text-align: center;
		line-height: 45px;
		margin: 30px auto;
		border-radius: 5px;
	}
</style>
