<template>
	<view class="content">
		<view class="log-box">
			<image class="log-img" src="/static/log.png" alt="">
		</view>
		<view class="phone-box">
			<view class="phone-left"></view>
			<view class="phone-centent">
				<view class="phone-title">手机号码</view>
				<view class="phone-input-box">
					<input type="text" v-model="myPhone" placeholder="输入手机号码">
				</view>
				<view class="phone-get"  @click="getCode" :style="style">
					{{ buttonInfo }}
				</view>
			</view>
			<view class="phone-right"></view>
		</view>
		<view class="phone-box">
			<view class="phone-left"></view>
			<view class="phone-centent">
				<view class="phone-title">验证码</view>
				<view class="phone-input-box2">
					<input type="text" v-model="code" placeholder="输入验证码" :style="verifyStyle" @click="clearCode">
				</view>
				<view class="phone-get-y">
				</view>
			</view>
			<view class="phone-right-y"></view>
		</view>
		<view class="submit-box">
			<view class="submit-left"></view>
				<view class="submit-centent" @click="verification">
					登录
				</view>
			<view class="submit-right"></view>
		</view>
		<view class="rule-box">
			<span class="rule-left">点击“登录”按钮即表示你同意</span>
			<span class="rule-right" @click="ruleShow">《解忧服务员合作协议》</span>
		</view>
		<view class="rule-info" @click="ruleNone" v-show="show">
			<h5>解忧服务员合作协议</h5>
			<p>
				游泳规则
				a swimming pool's regulations;
				交通规则
				traffic regulations;
				遵守规则
				follow the procedures;
				违反规则
				infringe rules;
				拼写规则
				rules of spelling;
				有规则必有例外。
				There is no rule without an exception.
				没有一成不变的规则。
				There is no cast-iron [fixed] rule.
				（整齐） regular:
				这条河流的水道原来很不规则。
				The course of this river used to be quite irregular.
				这一片建筑物的布局十分规则。
				The buildings in this area are laid out in an orderly fashion.
			</p>
		</view>
	</view>
</template>

<script>
	export default {
		
		data() {
			return {
				show: false,
				buttonInfo: '获取验证码',
				second:60,
				style: '',
				code: '',
				myPhone: '',
				verifyStyle: ''
			}
		},
		onLoad() {
			//获取跳转参数
			let userinfo = uni.getStorageSync('userinfo')
			if (userinfo) {
				uni.navigateTo({
				    url: '/pages/orderTaking/orderTaking'
				});
			}
		},
		methods: {
			ruleShow(){
				this.show = true
			},
			
			ruleNone(){
				this.show = false
			},
			
			getCode(){
				if (this.myPhone) {
					let exp = /^(13[0-9]|14[01456879]|15[0-35-9]|16[2567]|17[0-8]|18[0-9]|19[0-35-9])\d{8}$/
					if (!exp.test(this.myPhone)) {
						uni.showModal({
							title: '温馨提示',
							content: '请输入正确的手机号',
							success: function (res) {
								
							}
						});
					}
					uni.request({
						url: "http://test.qd-happy.com/app_service",
						method: "POST",
						header: {
							'Content-Type': "multipart/form-data",
						},
						data: {
							interface: "users_sendMsg",
							data: {
								phone: this.myPhone
							}
						},
						success: (res) => {
							console.log(res.data)
							if (res.error_code == '000000') {
								uni.showModal({
									title: '温馨提示',
									content: '短信已下发成功，请查收',
									success: function (res) {
										
									}
								});
							}
						}
					})
					
					var interval = setInterval(() => {
						let times = --this.second
						this.second = times<10?'0'+times:times //小于10秒补 0
						this.buttonInfo = this.second
						this.style = 'pointer-events: none'	
					}, 1000)
					
					setTimeout(() => {
						clearInterval(interval)
						this.second=60
						this.buttonInfo = '重新获取'
						this.style = ''
					}, 60000)
				} else {
					uni.showModal({
						title: '温馨提示',
						content: '请输入手机号',
						success: function (res) {
							
						}
					});
				}
			},
			
			verification(){
				if (!this.myPhone) {
					uni.showModal({
						title: '温馨提示',
						content: '请输入手机号',
						success: function (res) {
							
						}
					});
				}
				if (!this.code) {
					uni.showModal({
						title: '温馨提示',
						content: '请输入验证码',
						success: function (res) {
							
						}
					});
				}
				if (this.code && this.myPhone) {
					//缓存用户数据
					let userinfo = {}
					userinfo.user_id = 2
					userinfo.user_name = '小娟'
					userinfo.pic = '/static/images/headimg.jpeg'
					uni.setStorageSync('userinfo', userinfo)
					
					//获取骑手状态
					uni.request({
						url: "http://test.qd-happy.com/app_service",
						method: "POST",
						header: {
							'Content-Type': "multipart/form-data",
						},
						data: {
							interface: "users_getWorkingStatus",
							data: {
								user_id: userinfo.user_id
							}
						},
						success: (res) => {
							console.log(res)
							if (res.data.data.working_status == 1) {
								uni.navigateTo({
								    url: '/pages/orderTaking/orderTaking'
								});
							} else {
								uni.navigateTo({
								    url: '/pages/finishWork/finishWork'
								});
							}
						}
					})
					
					
					
					// uni.request({
					// 	url: "http://test.qd-happy.com/app_service",
					// 	method: "POST",
					// 	header: {
					// 		'Content-Type': "multipart/form-data",
					// 	},
					// 	data: {
					// 		interface: "users_userLogin",
					// 		data: {
					// 			phone: this.myPhone,
					// 			code: this.code
					// 		}
					// 	},
					// 	success: (res) => {
					// 		console.log(res.data)
					// 		if (res.data.error_code !== '000000') {
					// 			this.code = '验证码错误，请重新输入'
					// 			this.verifyStyle = "color: red"
					// 		} else {
					// 			//缓存用户数据
					// 			let userinfo = {}
					// 			userinfo.user_id = res.data.data.user_id
					// 			userinfo.user_name = res.data.data.user_name
					// 			userinfo.pic = res.data.data.pic
					// 			uni.setStorageSync('userinfo', userinfo)
								
					// 			uni.navigateTo({
					// 			    url: '/pages/orderTaking/orderTaking?user_id='+res.data.data.user_id+'&pic='+res.data.data.pic+'&user_name'+res.data.data.user_name
					// 			});
					// 		}
					// 	}
					// })
				}
			},
			
			clearCode(){
				this.code = ''
				this.verifyStyle = ''
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
		position: relative;
	}
	.log-box{
		width:100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		margin-top: 91px;
		margin-bottom: 30px;
	}
	.log-img{
		width: 96.5px;
		height: 100px;
	}
	.phone-box{
		width: 70%;
		height: 50px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		margin: 10px auto;
		position: relative;
	}
	.phone-left{
		width: 50px;
		height: 50px;
		border: 1px solid #eee;
		border-radius: 50%;
		position: absolute;
		left: -22px;
	}
	.phone-centent{
		width: 100%;
		height: 50px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		position: absolute;
		border-top: 1px solid #eee;
		border-bottom: 1px solid #eee;
		background-color: #fff;
		z-index: 20;
	}
	.phone-title{
		width: 33%;
		font-size: 12px;
	}
	.phone-input-box{
		width: 40%;
	}
	.phone-input-box2{
		width: 80%;
		font-size: 13px;
	}
	.phone-input-box>input{
		font-size: 14px;
	}
	.phone-get{
		width: 30%;
		height: 50px;
		font-size: 12px;
		line-height: 50px;
		color: #fff;
		text-align: right;
		background-color: #308bd1;
		right: 0px;
		text-align: center;
	}
	.phone-right{
		width: 48px;
		height: 50px;
		border: 1px solid #eee;
		border-radius: 50%;
		position: absolute;
		right: -22px;
		z-index: 19;
		background-color: #308bd1;
	}
	.phone-get-y{
		width: 30%;
		height: 50px;
		font-size: 12px;
		line-height: 50px;
		color: #fff;
		text-align: right;
		right: 0px;
	}
	.phone-right-y{
		width: 48px;
		height: 50px;
		border: 1px solid #eee;
		border-radius: 50%;
		position: absolute;
		right: -22px;
		z-index: 19;
	}
	.submit-box{
		width: 70%;
		height: 50px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		margin: 40px auto;
		position: relative;
	}
	.submit-centent{
		width: 100%;
		height: 50px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		position: absolute;
		border-top: 1px solid #eee;
		border-bottom: 1px solid #eee;
		background-color: #308bd1;
		z-index: 20;
		color: #fff;
	}
	.submit-left{
		width: 50px;
		height: 50px;
		border: 1px solid #eee;
		border-radius: 50%;
		position: absolute;
		left: -22px;
		background-color: #308bd1;
	}
	.submit-right{
		width: 48px;
		height: 50px;
		border: 1px solid #eee;
		border-radius: 50%;
		position: absolute;
		right: -22px;
		z-index: 19;
		background-color: #308bd1;
	}
	
	.rule-box{
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		height: 50px;
		width: 100%;
		margin-top: 170px;
	}
	.rule-left{
		font-size:10px;
		color: #CCCCCC;
	}
	.rule-right{
		font-size:10px;
		color: #7fa8e4;
	}	.rule-info{
		width: 100%;
		position: absolute;
		bottom: 0px;
		background-color: #eee;
		display: flex;
		z-index: 40;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding-bottom: 50px;
	}
	.rule-info>h5{
		line-height: 50px;
	}
</style>

