<template>
	<view class="content">
		<view class="main-top">
			<view class="top-back">
				<view class="top-left">
					<view class="back-box" @tap="navigateBack">
						<image src="/static/images/back.png" mode=""></image>
					</view>
				</view>
				<view class="top-right">
					
				</view>
			</view>
			<view class="top-info">
				<view class="info-left">
					<view class="left-line1">
						<view class="line1-name">
							订单已完成
						</view>
						<view class="line1-title">
							配送时间：40分钟
						</view>
						<view class="line1-arrive">
							到达时间：2018年6月22日15:25:34
						</view>
					</view>
				</view>
				<view class="info-right">
					<view class="right-headimg">
						<image src="/static/images/finish.png" mode=""></image>
					</view>
				</view>
			</view>
		</view>
		<view class="finish-info">
			<view class="info-top">
				<purchasingAgent :nameShow="nameShow"></purchasingAgent>
			</view>
			<view class="info-item2">
				<view class="item2-box">
					<view class="item2-left">
						<view class="item2-left-img">
							<image src="/static/images/number.png" mode=""></image>
						</view>
						<span class="item2-left-title">20931</span>
					</view>
					<view class="item2-right">
						<view class="item2-right-img">
							<image src="/static/images/phone.png" mode=""></image>
						</view>
					</view>
				</view>
			</view>
			<view class="order-title">
				标题：需要一份感冒药、食品
			</view>
			<view class="order-title">
				备注：到楼下打电话，别敲门
			</view>
		</view>
		<view class="finish-number">
			<view class="number-item">
				<view class="number-item-title">
					订单编号：274996818880393
				</view>
				<button class="number-item-button">复制</button>
			</view>
			<view class="number-item">
				<view class="number-item-title">
					订单编号：
					<button class="number-item-button2">帮买</button>
					<button class="number-item-button2">帮取送</button>
				</view>
				
			</view>
		</view>
		<view class="finish-number">
			<view class="number-item">
				<view class="number-item-title">
					总费用：
					<span class="number-item-span">￥280</span>
				</view>
			</view>
			<view class="number-item">
				<view class="number-item-title">
					分成收益：
					<span class="number-item-span">￥100</span>
				</view>
			</view>
		</view>
		<view class="finish-voucher">
			<view class="info-item4">
				凭证列表
			</view>
			<view class="info-item5">
				<view class="item5-img">	
					<image src="/static/images/yaopin.jpg" mode=""></image>
				</view>
				<view class="item5-img">
					<image src="/static/images/yaopin.jpg" mode=""></image>
				</view>
				<view class="item5-img">
					<image src="/static/images/yaopin.jpg" mode=""></image>
				</view>
				<view class="item5-img">
					<image src="/static/images/yaopin.jpg" mode=""></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import purchasingAgent from '../../components/purchasingAgent.vue'
	export default {
		components:{
			purchasingAgent
		},
		data() {
			return {
				nameShow:false
			}
		},
		onLoad(options){    //传入的参数名称，可自行修改
		console.log(options);    查看传入的id值
		methods: {
			navigateBack(){
				uni.navigateBack()
			//获取完成订单详情页面
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
							interface: "order_getOrderView",
							data: {
								user_id: userData.user_id,
								order_id: this.page
							}
						},
						success: (res) => {
							console.log(res,'这是已完成')
						}
					})
				}  
			}
			
		}
	},
	}
</script>

<style>
	body{
		background-color: #fafafa;
	}
	.item5-img{
		width: 80px;
		height: 80px;
		margin: 0 5px;
	}
	.item5-img>image{
		width: 100%;
		height: 100%;
	}
	.info-item5{
		width: 100%;
		height: 90px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}
	.info-item4{
		width: 92%;
		height: 30px;
		text-align: left;
		line-height: 30px;
		font-size: 15px;
		color: #333;
		font-weight: 600;
	}
	.finish-voucher{
		width: 100%;
		height: 140px;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		box-shadow:0px 5px 10px 0px #ccc;
		background-color: #fff;
		margin-top: 8px;
	}
	.number-item-span{
		font-size: 15px;
		font-weight: 600;
		color: #d13043;
	}
	.number-item-button2{
		width: 30%;
		height: 30px;
		background-color: #308bd1;
		color: #fff;
		line-height: 30px;
		border-radius: 30px;
	}
	.number-item-button{
		width: 20%;
		height: 30px;
		border: 1px solid #eee;
		border-radius: 5px;
		font-size: 14px;
		line-height: 30px;
		color: #333;
	}
	.number-item-title{
		font-size: 15px;
		color: #333;
		line-height: 40px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: flex-start;
	}
	.number-item{
		width: 92%;
		height: 40px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: flex-start;
		border-bottom: 1px solid #eee;
	}
	.finish-number{
		width: 100%;
		height: 80px;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		box-shadow:0px 5px 10px 0px #ccc;
		background-color: #fff;
		margin-top: 8px;
	}
	.order-title{
		width: 90%;
		height: 40px;
		font-size: 15px;
		color: #d13043;
		font-weight: 600;
		margin: 0 auto;
		line-height: 40px;
	}
	.item2-right-img{
		width: 23px;
		height: 23px;
	}
	.item2-right-img>image{
		width: 100%;
		height: 100%;
	}
	.item2-left-title{
		font-size: 12px;
		color: #308bd1;
		padding: 5px;
	}
	.item2-left-img{
		width: 23px;
		height: 23px;
	}
	.item2-left-img>image{
		width: 100%;
		height: 100%;
	}
	.info-top{
		width: 92%;
		height: auto;
	}
	.item2-label1{
		width: 50px;
		height: 20px;
		background-color: #308bd1;
		font-size: 12px;
		color: #fff;
		text-align: center;
		line-height: 20px;
		border-radius: 2px;
	}
	.item2-label2{
		width: 75px;
		height: 20px;
		background-color: #aaaaaa;
		font-size: 12px;
		color: #fff;
		text-align: center;
		line-height: 20px;
		border-radius: 2px;
		margin-left: 10px;
	}
	.item2-box{
		width: 92%;
		height: 35px;
		border-bottom: 1px solid #eee;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}
	.info-item2{
		width: 100%;
		height: auto;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}
	.item2-left{
		width: 74%;
		height: 30px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: flex-start;
		font-size: 12px;
		color: #333;
	}
	.item2-right{
		width: 20%;
		height: 30px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: flex-end;
		font-size: 12px;
		color: #308bd1;
	}
	.finish-info{
		width: 100%;
		height: 260px;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		box-shadow:0px 5px 10px 0px #ccc;
		background-color: #fff;
	}
	.content{
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		width: 100%;
	}
	.main-top{
		width: 100%;
		height: 195px;
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
	.top-info{
		width: 100%;
		height: 106px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}
	.info-left{
		width: 60%;
		height: 80px;
		display: flex;
		align-items: center;
		flex-direction: column;
		justify-content: flex-start;
	}
	.info-right{
		width: 30%;
		height: 60px;
		display: flex;
		flex-direction: row;
		justify-content: flex-end;
	}
	.left-line1{
		height: 30px;
		color: #fff;
	}
	.line1-name{
		font-size: 24px;
		line-height: 30px;
	}
	.line1-title{
		font-size: 17px;
		line-height: 30px;
	}
	.line1-arrive{
		font-size: 12px;
		line-height: 20px;
	}
	.right-headimg{
		width: 88px;
		height: 77px;
	}
	.right-headimg>image{
		width: 100%;
		height: 100%;
	}
</style>
