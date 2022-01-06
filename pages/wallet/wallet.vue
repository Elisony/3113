<template>
	<view class="content">
		<view class="main-top">
			<view class="top-back" @tap="navigateBack">
				<view class="top-left">
					<view class="back-box">
						<image src="/static/images/back.png" mode=""></image>
					</view>
				</view>
				<view class="top-right">
					我的钱包
				</view>
			</view>
			<view class="top-info">
				<view class="info-left">
					<view class="left-line1">
						<span class="line1-name">
							账户总额(元)
						</span>
					</view>
					<view  class="left-line2">
						{{WalletInfo.balance}}
					</view>
				</view>
				<view class="info-right">
					<view class="info-right-item1">
						<p class="item-title">今日收益(元)</p>
						<p class="item-content">+{{WalletInfo.work_sum}}</p>
					</view>
					<view class="info-right-item2">
						<p class="item-title">累计收益(元)</p>
						<p class="item-content">{{WalletInfo.account}}</p>
					</view>
				</view>
			</view>
		</view>
		<navigator url="/pages/balanceDetailed/balanceDetailed" hover-class="navigator-hover" class="wallet-bottom">
			收入明细
		</navigator>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				WalletInfo:{}
			}
		},
		onLoad() {
			this.getWallet()
		},
		methods: {
			navigateBack(){
				//uni.navigateBack()
				detail:-1
			},
			//获取钱包详情页面
			getWallet() {
				let userData = uni.getStorageSync('userinfo')
				if (userData) {
					uni.request({
						url: "http://test.qd-happy.com/app_service",
						method: "POST",
						header: {
							'Content-Type': "multipart/form-data",
						},
						data: {
							interface: "users_getAccountInfo",
							data: {
								user_id: userData.user_id
							}
						},
						success: (res) => {
							this.WalletInfo=res.data.data
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
		height: 240px;
		background-color: #308bd1;
		border-radius: 0 0 15px 15px;
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
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
	.info-left{
		width: 80%;
		height: 90px;
		display: flex;
		flex-direction: column;
		justify-content: center;
		margin-top: 40px;
	}
	.info-right{
		width: 80%;
		height: 60px;
		display: flex;
		flex-direction: row;
		justify-content: center;
		text-align: center;
		margin-top: 20px;
	}
	.left-line1{
		height: 30px;
		color: #fff;
		text-align: center;
	}
	.line1-name{
		font-size: 15px;
		line-height: 50px;
	}
	.line1-title{
		font-size: 12px;
		padding-left: 10px;
		padding-top: 5px;
	}
	.left-line2{
		height: 60px;
		color: #fff;
		font-size: 30px;
		font-weight: 600;
		text-align: center;
		line-height: 60px;
	}
	.info-right-item1{
		width: 50%;
		height: 60px;
	}
	.info-right-item2{
		width: 50%;
		height: 60px;
	}
	.item-title{
		font-size: 15px;
		color: #fff;
	}
	.item-content{
		font-size: 18px;
		color: #fff;
		font-weight: 600;
		line-height: 30px;
	}
	.wallet-bottom{
		width: 90%;
		height: 55px;
		background-color: #fff;
		position: fixed;
		bottom: 20px;
		border: 1px solid #eee;
		border-radius: 1px;
		display: flex;
		flex-direction: row;
		justify-content: center;
		line-height: 55px;
		font-size: 18px;
		color: #308bd1;
	}
</style>
