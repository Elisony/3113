<template>
	<view class="content">
		<view class="main-top">
			<view class="top-back">
				<view class="top-back-left">
					<navigator url="/pages/orderTaking/orderTaking" hover-class="navigator-hover" class="back-box">
						<image src="/static/images/back.png" mode=""></image>
					</navigator>
				</view>
				<view class="top-back-right">
					我的任务
				</view>
			</view>
		</view>
		<view class="option-box">
			<navigator url="/pages/completeTask/completeTask" hover-class="navigator-hover" class="option-left">
				已完成
			</navigator>
			<view class="option-right">
				已取消
			</view>
		</view>
		<view style="width: 100%;" v-for="(item,index) in taskList" :key="index">
			<view class="order-list">
				<view class="list-top">
					<view class="top-left">
						取消原因：
					</view>
					<view class="top-right">
						{{ item.cancel_msg }}
					</view>
				</view>
				<dispatchingInfo :item="item"></dispatchingInfo>
				<view class="order-title">
					<view class="order-title-type" v-for="(raw, key) in item.channel_list">
						{{ raw }}
					</view>
				</view>
				<view class="button-box">
					查看订单详情
				</view>
			</view>
			
			<!-- <view class="order-list">
				<view class="list-top">
					<view class="top-left">
						取消原因：
					</view>
					<view class="top-right">
						服务员车坏了，与客户沟通后取消订单。
					</view>
				</view>
				<purchasingAgent></purchasingAgent>
				<view class="order-title">
					<view class="order-title-type">
						代购订单
					</view>
				</view>
				<view class="button-box">
					查看订单详情
				</view>
			</view>
			
			<view class="order-list">
				<view class="list-top">
					<view class="top-left">
						取消原因：
					</view>
					<view class="top-right">
						服务员车坏了，与客户沟通后取消订单。
					</view>
				</view>
				<retail></retail>
				<view class="order-title">
					<view class="order-title-type">
						零售订单
					</view>
				</view>
				<view class="button-box">
					查看订单详情
				</view>
			</view> -->
		</view>
	</view>
</template>

<script>
	import dispatchingInfo from '../../components/dispatchingInfo.vue'
	import purchasingAgent from '../../components/purchasingAgent.vue'
	import retail from '../../components/retail.vue'
	export default {
		components:{
			dispatchingInfo,
			purchasingAgent,
			retail
		},
		data() {
			return {
				taskList: [],
				page: 1,
				maxPage: 0
			}
		},
		onLoad() {
			let then = this
			
			//获取骑手已取消订单
			then.getTask()
			
		},
		onReachBottom(){
			let then = this
			//获取骑手已取消订单
			then.getTask()
		},
		methods: {
			//获取骑手已取消订单
			getTask(){
				let userData = uni.getStorageSync('userinfo')
				if (userData) {
					uni.request({
						url: "http://test.qd-happy.com/app_service",
						method: "POST",
						header: {
							'Content-Type': "multipart/form-data",
						},
						data: {
							interface: "order_getCancelOrderList",
							data: {
								user_id: userData.user_id,
								page: this.page
							}
						},
						success: (res) => {
							this.taskList = this.taskList.concat(res.data.data.list)
							this.page += 1
							console.log(this.taskList)
						}
					})
				}  
			}
		}
	}
</script>

<style>
	body{
		background-color: #fafafa;
	}
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		width: 100%;
	}
	.main-top{
		width: 100%;
		height: 70px;
		background-color: #308bd1;
	}
	.top-back{
		width: 100%;
		height: 44px;
		margin-top: 20px;
		display: flex;
		flex-direction: row;
		line-height: 50px;
	}
	.top-back-left{
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
	.top-back-right{
		width: 58%;
		height: 30px;
		color: #fff;
		font-size: 16px;
	}
	.top-back-info{
		width: 100%;
		height: 106px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}
	.order-list{
		width: 100%;
		height: auto;
		box-shadow:0px 5px 10px 0px #ccc;
		margin-bottom: 10px;
	}
	.list-top{
		width: 100%;
		height: 60px;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: flex-start;
		border-bottom: 1px solid #eee;
		padding-top: 10px;
	}
	.top-left{
		width: 92%;
		height: 30px;
		font-size: 15px;
		font-weight: 600;
		color: #333;
		line-height: 20px;
	}
	.top-right{
		width: 92%;
		height: 30px;
		font-size: 12px;
		color: #333;
		font-weight: 600;
		line-height: 15px;
	}
	.list-address{
		width: 100%;
		height: 110px;
		border-bottom: 1px solid #eee;
	}
	.list-item{
		width: 100%;
		height: 30px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}
	.item-left{
		width: 70%;
		height: 40px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: flex-start;
	}
	.item-right{
		width: 22%;
		height: 40px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: flex-end;
		font-size: 15px;
		color: #308bd1;
	}
	.left-spot{
		font-size: 30px;
		color: #308bd1;
		padding-bottom: 20px;
	}
	.left-title{
		font-size: 15px;
		color: #333333;
		padding-left: 10px;
	}
	.item-down-box{
		width: 92%;
		height: 30px;
		display: flex;
		flex-direction: row;
		align-items: center;
	}
	.item-down{
		width: 8px;
		height: 14px;
	}
	.item-down>image{
		width: 100%;
		height: 100%;
	}
	.down-title{
		font-size: 12px;
		color: #ccc;
		padding-left: 10px;
	}
	.left-spot2{
		font-size: 30px;
		color: red;
		padding-bottom: 20px;
	}
	.order-title{
		width: 100%;
		height: 40px;
		border-bottom: 1px solid #eee;
	}
	.order-title-type{
		width: 80px;
		height: 25px;
		background-color: #308bd1;
		font-size: 12px;
		color: #fff;
		margin: 0 auto;
		line-height: 25px;
		text-align: center;
		border-radius: 30px;
		margin-left: 2%;
		float: left;
		margin-top: 8px;
	}
	.button-box{
		width: 100%;
		height: 50px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		color: #308bd1;
		font-size: 15px;
		font-weight: 600;
	}
	.option-box{
		width: 100%;
		height: 50px;
		border-bottom: 1px solid #eee;
		margin-bottom: 10px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: space-around;
	}
	.option-left{
		width: 48%;
		height: 30px;
		border-right: 1px solid #ccc;
		font-size: 15px;
		color: #ccc;
		text-align: center;
		line-height: 30px;
	}
	.option-right{
		width: 50%;
		font-size: 15px;
		color: #308bd1;
		text-align: center;
		height: 30px;
		line-height: 30px;
	}
</style>
