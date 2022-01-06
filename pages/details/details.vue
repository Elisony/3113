<template>
	<view class="content">
		<view class="top-box"></view>
		<view class="swiper-box">
			<swiper class="swiper" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration" indicator-color="#fff" indicator-active-color="#308bd1">
				<swiper-item class="swiper-item" v-for="(item, index) in details.pic_list" :key="index">
					<image v-if="item" :src="item" mode=""></image>
				</swiper-item>
			</swiper>
		</view>
		<view class="details-info">
			<view class="info-item1">
				<view class="item1-title">
					{{ details.type_names }}
				</view>
				<view class="item1-describe">
					备注：{{ details.content }}
				</view>
			</view>
			<!-- <view class="info-item2">
				<view class="item2-box">
					<view class="item2-left">
						指定位置：山东省青岛市市南区山东路6号万象城
					</view>
					<view class="item2-right">
						修改位置
					</view>
				</view>
			</view> -->
			<view class="info-item2">
				<view class="item2-box">
					<view class="item2-left">
						总样数
					</view>
					<view class="item2-right-color">
						{{ details.num }}
					</view>
				</view>
			</view>
			<view class="info-item2">
				<view class="item2-box">
					<view class="item2-left">
						<view class="item2-label1" @click="shiftTypes('易碎')" v-if="types.indexOf('易碎') != -1">易碎</view>
						<view class="item2-label2" @click="pushTypes('易碎')" v-if="types.indexOf('易碎') == -1">易碎</view>
						<view class="item2-label1" @click="shiftTypes('超重 超大')" v-if="types.indexOf('超重 超大') != -1">超重 超大</view>
						<view class="item2-label2" @click="pushTypes('超重 超大')" v-if="types.indexOf('超重 超大') == -1">超重 超大</view>
					</view>
					<view class="item2-right-color" @click="updateOverAccount">
						￥{{ details.over_account }}
					</view>
				</view>
			</view>
			<view class="info-item2">
				<view class="item2-box">
					<view class="item2-left">
						商品垫付价格
					</view>
					<view class="item2-right-color" v-if="details.info_account != '0.00'"  @click="updateInfoAccount">
						￥{{ details.info_account }}
					</view>
					<view class="item2-right-color" v-else  @click="updateInfoAccount">
						请输入垫付金额
					</view>
				</view>
			</view>
			<view class="info-item3">
				<button class="item-button1" @click="getInfoTiming">计时：{{ details.timing }}分钟</button>
				<button class="item-button2" v-if="details.status != '0'" @click="uploadVoucher">上传凭据</button>
				<button class="item-button2-none" v-else>上传凭据</button>
			</view>
			<view class="info-item4">
				*如果存在恶意计时的问题，则骑手需承担百倍罚款
			</view>
			<view class="info-item5">
				<view class="item5-img" v-for="(raw, key) in details.upload_list" :key="key">	
					<image v-if="raw" :src="raw" mode=""></image>
				</view>
			</view>
			<!-- <view class="info-item5">
				<view class="item5-img">	
					<image src="/static/images/yaopin.jpg" mode=""></image>
				</view>
			</view> -->
			<view class="details-bottom">
				<view class="details-bottom-button" v-if="details.status == '0'" @click="updateStatus(details.status)">
					到达取货点
				</view>
				<view class="details-bottom-button" v-else-if="details.status == '1'" @click="updateStatus(details.status)">
					清点样数，物件检查
				</view>
				<view class="details-bottom-button" v-else-if="details.status == '2'" @click="updateStatus(details.status)">
					到达取货点
				</view>
				<view class="details-bottom-button" v-else-if="details.status == '3'" @click="updateStatus(details.status)">
					已买 已取
				</view>
				<view class="details-bottom-button" v-else-if="details.status == '4'" @click="updateStatus(details.status)">
					返回列表
				</view>
			</view>
		</view>
		<view class="alert-box" :style="{height: QHeight+'px'}" v-show="overShow">
			<view class="alert-colose"  @click="ruleNone('over')">
				X
			</view>
			<view class="alert-info">
				<view class="alert-info-title">
					请输入服务代付超出金额
				</view>
				<view class="alert-info-content">
					<input type="text" class="alert-input" v-model="overAccount">
				</view>
				<bitton class="alert-info-button" @tap="submitOverAccount">提交</bitton> 
			</view>
		</view>
		<view class="alert-box" :style="{height: QHeight+'px'}" v-show="show">
			<view class="alert-colose"  @click="ruleNone('info')">
				X
			</view>
			<view class="alert-info">
				<view class="alert-info-title">
					请输入服务代付金额
				</view>
				<view class="alert-info-content">
					<input type="text" class="alert-input" v-model="infoAccount">
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
				indicatorDots: true,
				autoplay: true,
				interval: 2000,
				duration: 500,
				info_id: 0,
				order_id: 0,
				details: {},
				voucher: {},
				types: [],
				show: false,
				overShow: false,
				QHeight: 0,
				overAccount: null,
				infoAccount: null
			}
		},
		onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
			
			let then = this
			then.info_id = option.info_id
			then.order_id = option.order_id
			
			//获取骑手跑腿中详情页面
			then.getOrderInfoPro()
			
			//获取屏幕高度
			uni.getSystemInfo({
				success: (res) => {
					let height = res.windowHeight - uni.upx2px(0)
					then.QHeight = height
				}
			})
			
		},
		methods: {
			//获取骑手跑腿中详情页面
			getOrderInfoPro(){
				let userData = uni.getStorageSync('userinfo')
				if (userData) {
					uni.request({
						url: "http://test.qd-happy.com/app_service",
						method: "POST",
						header: {
							'Content-Type': "multipart/form-data",
						},
						data: {
							interface: "order_getOrderInfoPro",
							data: {
								user_id: userData.user_id,
								order_id: this.order_id,
								info_id: this.info_id
							}
						},
						success: (res) => {
							this.details = res.data.data
							if (this.details.over_account) {
								this.overAccount = this.details.over_account
							}
							if (this.details.over_account) {
								this.overAccount = this.details.over_account
							}
							
							this.types = this.details.over_type
							console.log(this.details)
						}
					})
				}  
			},
			
			//修改物品状态
			updateStatus(res){
				if (res == 4) {
					uni.navigateTo({
						url: '/pages/orderReceived/orderReceived?id='+this.order_id
					});
				} else {
					let userData = uni.getStorageSync('userinfo')
					let status = parseInt(res) + 1
					uni.request({
						url: "http://test.qd-happy.com/app_service",
						method: "POST",
						header: {
							'Content-Type': "multipart/form-data",
						},
						data: {
							interface: "order_upInfoStatus",
							data: {
								user_id: userData.user_id,
								order_id: this.order_id,
								info_id: this.info_id,
								status: status
							}
						},
						success: (res) => {
							uni.redirectTo({
								url: '/pages/details/details?info_id='+this.info_id+'&order_id='+this.order_id
							});
							
						}
					})
				}
				
			},
			
			//上传凭证
			uploadVoucher(){
				let userData = uni.getStorageSync('userinfo')
				let then = this
				uni.chooseImage({
				    count: 6, //默认9
				    sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
				    sourceType: ['album'], //从相册选择
				    success: function (res) {
						const tempFilePaths = res.tempFilePaths
						var param = {
							interface: "order_uploadPic",
							data: JSON.stringify({
								user_id: userData.user_id,
								order_id: then.order_id,
								info_id: then.info_id,
							}),
						}
						uni.uploadFile({
							url: 'http://test.qd-happy.com/app_service', //仅为示例，非真实的接口地址
							filePath: tempFilePaths[0],
							fileType:'image',
							name: 'file',
							formData: param,
							header: {
								"Content-Type": "multipart/form-data",
								// "Accept": "application/json"
							},
							success: (uploadFileRes) => {
								uni.showToast({
									title: res.data.res,  
									icon: 'none'  
								});  
								uni.redirectTo({
									url: '/pages/details/details?info_id='+this.info_id+'&order_id='+this.order_id
								});
								
							}
						});
				    }
				});
			},
			
			//选中type值
			pushTypes(res){
				this.types.push(res)
			},
			
			//移除type值
			shiftTypes(res){
				for(let i = 0; i < this.types.length; i++) {
					if (res == this.types[i]) {
						console.log(i)
						this.types.splice(i, 1)
					}
				}
			},
			
			//弹出服务代付超出金额
			updateOverAccount(){
				this.overShow = true
			},
			
			//修改服务代付超出金额
			submitOverAccount(){
				let userData = uni.getStorageSync('userinfo')
				
				if(this.overAccount){
					uni.request({
						url: "http://test.qd-happy.com/app_service",
						method: "POST",
						header: {
							'Content-Type': "multipart/form-data",
						},
						data: {
							interface: "order_upOverAccount",
							data: {
								user_id: userData.user_id,
								order_id: this.order_id,
								info_id: this.info_id,
								types: this.types,
								price: this.overAccount
							}
						},
						success: (res) => {
							uni.showToast({
								title: res.data.res,  
								icon: 'none'  
							});  
							uni.redirectTo({
								url: '/pages/details/details?info_id='+this.info_id+'&order_id='+this.order_id
							});
							
						}
					})
				}
			},
			
			//弹出服务代付金额
			updateInfoAccount(){
				console.log(111111111)
				this.show = true
			},
			
			//修改服务代付金额
			submitInfoAccount(){
				let userData = uni.getStorageSync('userinfo')
				
				if(this.infoAccount){
					uni.request({
						url: "http://test.qd-happy.com/app_service",
						method: "POST",
						header: {
							'Content-Type': "multipart/form-data",
						},
						data: {
							interface: "order_upInfoAccount",
							data: {
								user_id: userData.user_id,
								order_id: this.order_id,
								info_id: this.info_id,
								price: this.infoAccount
							}
						},
						success: (res) => {
							uni.showToast({
								title: res.data.res,  
								icon: 'none'  
							});  
							uni.redirectTo({
								url: '/pages/details/details?info_id='+this.info_id+'&order_id='+this.order_id
							});
						}
					})
				}
			},
			
			//获取计时时间
			getInfoTiming(){
				let userData = uni.getStorageSync('userinfo')
				
				if(this.overAccount){
					uni.request({
						url: "http://test.qd-happy.com/app_service",
						method: "POST",
						header: {
							'Content-Type': "multipart/form-data",
						},
						data: {
							interface: "order_getInfoTiming",
							data: {
								user_id: userData.user_id,
								order_id: this.order_id,
								info_id: this.info_id
							}
						},
						success: (res) => {
							uni.showToast({
								title: res.data.res,  
								icon: 'none'  
							});  
							this.details.timing = res.data.data.timing
							
						}
					})
				}
			},
			
			ruleNone(res){
				if (res == 'over') {
					this.overShow = false
				} else {
					this.show = false
				}
			},
		}
	}
</script>

<style>
	body{
		background-color: #fafafa;
	}
	
	.details-bottom{
		width: 100%;
		height: 50px;
		background-color: #308bd1;
		font-size: 15px;
		color: #fff;
		position: fixed;
		bottom: 0px;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		
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
	}
	.info-item4{
		width: 100%;
		height: 30px;
		text-align: center;
		line-height: 30px;
		font-size: 12px;
		color: #d13043;
	}
	.item-button1{
		width: 30%;
		height: 30px;
		background-color: #308bd1;
		color: #fff;
		font-size: 12px;
	}
	.item-button2{
		width: 60%;
		height: 30px;
		background-color: #308bd1;
		color: #fff;
		font-size: 12px;
	}
	.item-button2-none{
		width: 60%;
		height: 30px;
		background-color: #308bd1;
		color: #fff;
		font-size: 12px;
		pointer-events: none;
	}
	.info-item3{
		width: 100%;
		height: 45px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: space-around;
	}
	.item2-label1{
		width: 100%;
		height: 20px;
		background-color: #308bd1;
		font-size: 12px;
		color: #fff;
		text-align: center;
		line-height: 20px;
		border-radius: 2px;
		padding: 0 10px;
		margin-left: 10px;
		white-space:nowrap
	}
	.item2-label2{
		width: 100%;
		height: 20px;
		background-color: #aaaaaa;
		font-size: 12px;
		color: #fff;
		text-align: center;
		line-height: 20px;
		border-radius: 2px;
		margin-left: 10px;
		padding: 0 10px;
		white-space:nowrap
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
		width: 80%;
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
	.item2-right-color{
		width: 26%;
		height: 30px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: flex-end;
		font-size: 12px;
		color: #333;
	}
	.item1-describe{
		width: 92%;
		height: 45px;
		font-size: 12px;
		color: #333;
		border-bottom: 1px solid #eee;
	}
	.item1-title{
		width: 92%;
		height: 30px;
		font-size: 15px;
		color: #333;
		font-weight: 600;
		padding-top: 20px;
	}
	.info-item1{
		width: 100%;
		height: auto;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
	.details-info{
		width: 100%;
		height: auto;
		box-shadow:0px 5px 10px 0px #ccc;
		padding-bottom: 10px;
		margin-bottom: 50px;
	}
	.swiper-box{
		width: 100%;
		height: 280px;
	}
	.swiper{
		width: 100%;
		height: 280px;
	}
	.swiper-item{
		width: 100%;
		height: 280px;
	}
	.swiper-item>image{
		width: 100%;
		height: 100%;
	}
	.top-box{
		width: 100%;
		height: 40px;
		background-color: #308bd1;
	}
	.content{
		width: 100%;
		height: 100%;
	}
	.alert-box{
		width: 100%;
		height: 300px;
		background-color: rgba(0,0,0,.5);
		position: fixed;
		top: 0px;
		z-index: 20;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}
	.alert-info{
		width: 300px;
		height: 200px;
		background-color: #fff;
		border-radius: 5px;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
	.alert-info-title{
		font-size: 20px;
		color: #333;
		font-weight: 600;
	}
	.alert-info-content{
		font-size: 15px;
		color: #333;
		font-weight: 600;
		line-height: 40px;
	}
	.alert-info-button{
		width: 30%;
		height: 30px;
		background-color: #308bd1;
		color: #fff;
		text-align: center;
		line-height: 30px;
		border-radius: 1px;
		margin-top: 10px;
	}
	.alert-input{
		width: 100%;
		height: 30px;
		border: 1px solid #ccc;
		margin: 10px 0;
	}
	.alert-colose{
		position: absolute;
		top: 10px;
		right: 10px;
		font-size: 25px;
		color: #fff;
	}
</style>
