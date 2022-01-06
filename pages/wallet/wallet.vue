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
					<view class="left-line2" @click="withdrawalOfbalance">
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
		<view class="alert-box" :style="{height: QHeight+'px'}" v-show="show">
			<view class="alert-colose" @click="ruleNone()">
				X
			</view>
			<view class="alert-info">
				<view class="alert-info-title">
					请输入提现金额
				</view>
				<view class="alert-info-content">
					<input type="number" class="alert-input" v-model="infoAccount">
				</view>
				<view>
					提现金额范围为：{{cashMinOrMax.cash_min}}元 ~ {{cashMinOrMax.cash_max}}元
				</view>
				<bitton class="alert-info-button" @tap="submitInfoAccount">提交</bitton>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				WalletInfo: {},
				QHeight: 0,
				show: false,
				infoAccount: '',
				cashMinOrMax: {}
			}
		},
		onLoad() {
			this.getWallet()
			//获取屏幕高度
			uni.getSystemInfo({
				success: (res) => {
					let height = res.windowHeight - uni.upx2px(0)
					this.QHeight = height
				}
			})
		},
		methods: {
			navigateBack() {
				uni.navigateBack()
				detail: -1
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
							this.WalletInfo = res.data.data
						}
					})
				}
			},
			//余额提现
			withdrawalOfbalance() {
				console.log(this.WalletInfo.balance)

				uni.request({
					url: "http://test.qd-happy.com/app_service",
					method: "POST",
					header: {
						'Content-Type': "multipart/form-data",
					},
					data: {
						interface: "users_getCashSet",
						data: {}
					},
					success: (res) => {
						this.cashMinOrMax = res.data.data
						if (this.WalletInfo.balance === '0.00' || Number(this.WalletInfo.balance) < Number(this
								.cashMinOrMax.cash_min)) {
							uni.showToast({
								title: '暂不满足提现要求',
								duration: 2000,
								icon: 'none'
							});
						} else {
							this.show = true
						}

					}
				})
			},
			ruleNone() {
				this.show = false
				this.infoAccount = ''
			},
			submitInfoAccount() {
				if (!this.infoAccount) {
					uni.showToast({
						title: '请输入提现金额！',
						duration: 2000,
						icon: 'none'
					});
				} else {

					let userData = uni.getStorageSync('userinfo')
					console.log(this.infoAccount, userData.user_id)
					if (userData) {
						uni.request({
							url: "http://test.qd-happy.com/app_service",
							method: "POST",
							header: {
								'Content-Type': "multipart/form-data",
							},
							data: {
								interface: "users_addCash",
								data: {
									user_id: userData.user_id,
									price: this.infoAccount
								}
							},
							success: (res) => {
								console.log(res)
								this.ruleNone()
								uni.showToast({
									title: res.data.code_message,
									duration: 2000,
									icon: 'none'
								});
							}
						})
					}
				}
			}
		}
	}
</script>

<style>
	body {
		background-color: #fafafa;
	}

	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		width: 100%;
	}

	.main-top {
		width: 100%;
		height: 240px;
		background-color: #308bd1;
		border-radius: 0 0 15px 15px;
	}

	.top-back {
		width: 100%;
		height: 44px;
		margin-top: 20px;
		display: flex;
		flex-direction: row;
		line-height: 50px;
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

	.top-info {
		width: 100%;
		height: 106px;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.info-left {
		width: 80%;
		height: 90px;
		display: flex;
		flex-direction: column;
		justify-content: center;
		margin-top: 40px;
	}

	.info-right {
		width: 80%;
		height: 60px;
		display: flex;
		flex-direction: row;
		justify-content: center;
		text-align: center;
		margin-top: 20px;
	}

	.left-line1 {
		height: 30px;
		color: #fff;
		text-align: center;
	}

	.line1-name {
		font-size: 15px;
		line-height: 50px;
	}

	.line1-title {
		font-size: 12px;
		padding-left: 10px;
		padding-top: 5px;
	}

	.left-line2 {
		height: 60px;
		color: #fff;
		font-size: 30px;
		font-weight: 600;
		text-align: center;
		line-height: 60px;
	}

	.info-right-item1 {
		width: 50%;
		height: 60px;
	}

	.info-right-item2 {
		width: 50%;
		height: 60px;
	}

	.item-title {
		font-size: 15px;
		color: #fff;
	}

	.item-content {
		font-size: 18px;
		color: #fff;
		font-weight: 600;
		line-height: 30px;
	}

	.wallet-bottom {
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

	.alert-box {
		width: 100%;
		height: 300px;
		background-color: rgba(0, 0, 0, .5);
		position: fixed;
		top: 0px;
		z-index: 20;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}

	.alert-info {
		width: 300px;
		height: 200px;
		background-color: #fff;
		border-radius: 5px;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.alert-info-title {
		font-size: 20px;
		color: #333;
		font-weight: 600;
	}

	.alert-info-content {
		font-size: 15px;
		color: #333;
		font-weight: 600;
		line-height: 40px;
	}

	.alert-info-button {
		width: 30%;
		height: 30px;
		background-color: #308bd1;
		color: #fff;
		text-align: center;
		line-height: 30px;
		border-radius: 1px;
		margin-top: 10px;
	}

	.alert-input {
		width: 80%;
		height: 30px;
		border: 1px solid #ccc;
		margin: 10px auto;

	}

	.alert-colose {
		position: absolute;
		top: 10px;
		right: 10px;
		font-size: 25px;
		color: #fff;
	}
</style>
