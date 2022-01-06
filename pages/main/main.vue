<template>
	<view class="content">
		<view class="main-top">
			<navigator url="/pages/orderTaking/orderTaking" hover-class="navigator-hover" class="top-back">
				<view class="top-left">
					<view class="back-box">
						<image src="/static/images/back.png" mode=""></image>
					</view>
				</view>
				<view class="top-right">
					个人中心
				</view>
			</navigator>
			<view class="top-info">
				<view class="info-left">
					<view class="left-line1">
						<span class="line1-name">
							小童
						</span>
						<span class="line1-title">
							自营配送员
						</span>
					</view>
					<view  class="left-line2">
						13455668821
					</view>
				</view>
				<view class="info-right">
					<view class="right-headimg">
						<image src="/static/images/headimg.jpeg" mode=""></image>
					</view>
					<view class="right-more">
						<image src="/static/images/more.png" mode=""></image>
					</view>
				</view>
			</view>
		</view>
		<viw class="main-center">
			<navigator url="/pages/wallet/wallet" hover-class="navigator-hover" class="wallet-box" >
				<view class="wallet-left">
					<view class="wallet-img-box">
						<image src="/static/images/wallet.png" mode=""></image>
					</view>
					<span class="wallet-title">我的钱包</span>
				</view>
				<view class="wallet-right">
					15999
				</view>
			</navigator>
			<view class="achievement">
				<view class="achievement-title">
					本日成就
				</view>
				<view class="achievement-info">
					<view class="ach-info-item">
						<view class="item-img-box">
							<image src="/static/images/order.png" mode=""></image>
						</view>
						<p class="item-font">完成单量</p>
						<p class="item-num">19</p>
					</view>
					<view class="ach-info-item">
						<view class="item-img-box">
							<image src="/static/images/mileage.png" mode=""></image>
						</view>
						<p class="item-font">配送里程</p>
						<p class="item-num">100KM</p>
					</view>
					<view class="ach-info-item">
						<view class="item-img-box">
							<image src="/static/images/income.png" mode=""></image>
						</view>
						<p class="item-font">收入</p>
						<p class="item-num">189元</p>
					</view>
				</view>
			</view>
			<view class="service">
				<view class="wallet-left">
					<view class="wallet-img-box">
						<image src="/static/images/server.png" mode=""></image>
					</view>
					<span class="wallet-title">联系客服</span>
				</view>
				<view class="wallet-right">
					<view class="more-box">
						<image src="/static/images/more-2.png" mode=""></image>
					</view>
				</view>
			</view>
			<view class="service">
				<view class="wallet-left">
					<view class="wallet-img-box">
						<image src="/static/images/set.png" mode=""></image>
					</view>
					<span class="wallet-title">设置中心</span>
				</view>
				<view class="wallet-right">
					<view class="more-box">
						<image src="/static/images/more-2.png" mode=""></image>
					</view>
				</view>
			</view>
			<view class="service">
				<view class="wallet-left">
					<view class="wallet-img-box">
						<image src="/static/images/set.png" mode=""></image>
					</view>
					<span class="wallet-title">今日值班</span>
				</view>
				<view class="wallet-right">
					<view class="more-box">
						<image src="/static/images/more-2.png" mode=""></image>
					</view>
				</view>
			</view>
		</viw>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		onLoad() {
		this.getPersonalCenter()
		},
		methods: {
			//获取个人中心详情
			getPersonalCenter(){
				let userData = uni.getStorageSync('userinfo')
				if (userData) {
					uni.request({
						url: "http://test.qd-happy.com/app_service",
						method: "POST",
						header: {
							'Content-Type': "multipart/form-data",
						},
						data: {
							interface: "users_getUserInfo",
							data: {
								user_id: userData.user_id,
							}
						},
						success: (res) => {
							console.log(res,'这是个人中心')
						}
					})
				}  
			}
		}
	}
</script>

<style>
	.content {
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
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}
	.info-left{
		width: 40%;
		height: 60px;
	}
	.info-right{
		width: 40%;
		height: 60px;
		display: flex;
		flex-direction: row;
		justify-content: flex-end;
	}
	.left-line1{
		height: 30px;
		display: flex;
		align-items: center;
		flex-direction: row;
		color: #fff;
	}
	.line1-name{
		font-size: 16px;
	}
	.line1-title{
		font-size: 12px;
		padding-left: 10px;
		padding-top: 5px;
	}
	.left-line2{
		height: 30px;
		color: #fff;
		font-size: 12px;
	}
	.right-headimg{
		width: 60px;
		height: 60px;
		border-radius: 50%;
		overflow: hidden;
	}
	.right-headimg>image{
		width: 100%;
		height: 100%;
	}
	.right-more{
		width: 7px;
		height: 13px;
		line-height: 60px;
		margin-left: 10px;
	}
	.right-more>image{
		width: 100%;
		height: 100%;
	}
	.main-center{
		width: 100%;
		height: 300px;
		display: flex;
		flex-direction: column;
		align-items: center;
		position: relative;
		z-index: 19;
	}
	.wallet-box{
		width: 94%;
		height: 50px;
		position: absolute;
		top: -25px;
		z-index: 20;
		background-color: #fff;
		border-radius: 5px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		border: 1px solid #f1f1f1;
		box-shadow:0px 15px 10px -15px #ccc;
	}
	.wallet-left{
		width: 43%;
		height: 50px;
		display: flex;
		flex-direction: row;
		justify-content: flex-start;
		align-items: center;
	}
	.wallet-right{
		width: 43%;
		height: 50px;
		display: flex;
		flex-direction: row;
		justify-content: flex-end;
		align-items: center;
		font-size: 18px;
		color: #333;
	}
	.wallet-img-box{
		width: 24px;
		height: 28px;
	}
	.wallet-img-box>image{
		width: 100%;
		height: 100%;
	}
	.wallet-title{
		font-size: 15px;
		color: #333;
		padding-left: 10px;
		font-weight: 600;
	}
	.achievement{
		width: 94%;
		height: 152px;
		background-color: #fff;
		border-radius: 5px;
		margin-top: 30px;
		box-shadow:0px 15px 10px -15px #ccc;
		display: flex;
		flex-direction: column;
		align-items: center;
		border: 1px solid #f1f1f1;
	}
	.achievement-title{
		width: 67%;
		height: 50px;
		font-size: 15px;
		color: #333;
		font-weight: 600;
		line-height: 50px;
	}
	.achievement-info{
		width: 86%;
		height: 90px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: space-between;
	}
	.ach-info-item{
		width: 30%;
		height: 75px;
		display: flex;
		flex-direction: column;
		align-items: center;
	}
	.item-img-box{
		width: 24px;
		height: 28px;
	}
	.item-img-box>image{
		width: 100%;
		height: 100%;
	}
	.item-font{
		font-size: 15px;
		color: #333;
		line-height: 30px;
	}
	.item-num{
		font-size: 15px;
		color: #333;
		font-weight: 600;
	}
	.service{
		width: 94%;
		height: 50px;
		background-color: #fff;
		border-radius: 5px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		box-shadow:0px 15px 10px -15px #ccc;
		margin-top: 5px;
		border: 1px solid #f1f1f1;
	}
	.more-box{
		width: 7px;
		height: 13px;
	}
	.more-box>image{
		width: 100%;
		height: 100%;
		display: block;
	}
</style>
