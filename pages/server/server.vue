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
					服务
				</view>
			</view>
		</view>
		<view class="server-info">
			<view class="server-select">
				<view class="search-left">
					选择服务
				</view>
				<view class="search-right">
					<view class="search-right-select">
						<picker @change="bindPickerChange" :value="index" :range="chioseList">
							<view class="uni-input">{{chioseList[index]}}</view>
						</picker>
					</view>
					<view class="search-right-title">
						<view class="search-right-more">
							<image src="/static/images/more-2.png" mode=""></image>
						</view>
					</view>
				</view>
			</view>
			<view class="select-title">
				<view style="width: 100%;" v-for="(item, index) in proList" :key="index">
					<view class="server-title-none" @click="chiosePro(item.id)"  v-if="proIds.indexOf(item.id) == -1">
						{{ item.name }}
					</view>
					<view class="server-title" @click="shiftPro(item.id)" v-else-if="proIds.indexOf(item.id) != -1">
						{{ item.name }}
					</view>
				</view>
			</view>
		</view>
		<view class="server-info">
			<view class="server-select">
				<view class="search-left">
					总样数
				</view>
				<view class="search-right">
					<view class="search-right-select">
						<input type="text" placeholder="请输入总样数" v-model="proNum">
					</view>
				</view>
			</view>
		</view>
		<view class="details-bottom" @click="submitPro">
			提交
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				channelList: [],
				index: 0,
				chioseList: [],
				proList: [],
				proIds:[],
				proNum: null,
				order_id: null,
				chioseId: 1
			}
		},
		onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
			
			let then = this
			then.order_id = option.order_id
			
			//获取服务大类列表
			then.getChannelList()
			
			//获取服务列表
			this.getProList(1)
			
		},
		methods: {
			//获取选中元素索引
			bindPickerChange: function(e) {
				this.index = e.target.value
				
				for (let i = 0; i < this.channelList.length; i++) {
					if (this.channelList[i].name == this.chioseList[this.index]){
						this.chioseId = this.channelList[i].id
					}
				}
				
				//获取服务列表
				this.getProList(this.chioseId)
			},
			//返回
			navigateBack(){
				uni.navigateBack()
			},
			
			//获取服务大类列表
			getChannelList(){
				uni.request({
					url: "http://test.qd-happy.com/app_service",
					method: "POST",
					header: {
						'Content-Type': "multipart/form-data",
					},
					data: {
						interface: "order_getChannelList",
						data: {
							
						}
					},
					success: (res) => {
						let list = res.data.data.list
						this.channelList = list
						let newList = list.map(item => {
							return item.name;
						}).join(',').split(',');
						this.chioseList = newList
						
					}
				})
			},
			
			//获取服务列表
			getProList(channel_id) {
				uni.request({
					url: "http://test.qd-happy.com/app_service",
					method: "POST",
					header: {
						'Content-Type': "multipart/form-data",
					},
					data: {
						interface: "order_getProList",
						data: {
							channel_id: channel_id
						}
					},
					success: (res) => {
						this.proList = res.data.data.list
					}
				})
			},
			
			//选中服务项
			chiosePro(el){
				this.proIds.push(el)
			},
			
			//移除选中服务项
			shiftPro(el){
				for(let i = 0; i < this.proIds.length; i++) {
					if (el == this.proIds[i]) {
						console.log(i)
						this.proIds.splice(i, 1)
					}
				}
			},
			
			//提交临加服务项
			submitPro(){
				let userData = uni.getStorageSync('userinfo')
				if (userData) {
					uni.request({
						url: "http://test.qd-happy.com/app_service",
						method: "POST",
						header: {
							'Content-Type': "multipart/form-data",
						},
						data: {
							interface: "order_getProList",
							data: {
								user_id: userData.user_id,
								order_id: this.order_id,
								channel_id: this.chioseId,
								prolist: this.proIds,
								num: this.proNum
							}
						},
						success: (res) => {
							uni.showToast({
								title: res.data.res,  
								icon: 'none'  
							});  
							if (res.error_code == '000000') {
								uni.redirectTo({
								    url: '/pages/orderReceived/orderReceived?id='+this.order_id
								});
							}
							
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
		width: 45%;
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
		width: 45%;
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
	.server-info{
		width: 100%;
		height: 100%;
		background-color: #fff;
		border-bottom: 1px solid #eee;
		display: flex;
		flex-direction: column;
		justify-content: center;
		margin-bottom: 10px;
	}
	.server-select{
		width: 92%;
		height: 50px;
		border-bottom: 1px solid #eee;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		margin: auto;
	}
	.search-left{
		width: 28%;
		height: 30px;
		text-align: left;
		border-right: 1px solid #ccc;
		font-size: 18px;
		color: #333;
		font-weight: 600;
		line-height: 30px;
	}
	.search-right{
		width: 62%;
		height: 30px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: space-around;
	}
	.search-right-title{
		width: 20%;
		height: 30px;
		line-height: 30px;
		color: #ccc;
		display: flex;
		flex-direction: row;
		justify-content: flex-end;
	}
	.search-right-select{
		width: 80%;
		height: 30px;
		padding-left: 10px;
		line-height: 30px;
		color: #999;
	}
	.search-right-more{
		width: 7px;
		height: 13px;
	}
	.search-right-more>image{
		width: 100%;
		height: 100%;
	}
	.select-title{
		width: 92%;
		height: 67px;
		display: flex;
		flex-direction: row;
		align-items: center;
		margin: auto;
	}
	.server-title{
		width: 65px;
		background-color: 30px;
		background-color: #308bd1;
		font-size: 12px;
		color: #fff;
		line-height: 30px;
		text-align: center;
		border-radius: 30px;
		margin-left: 1px;
	}
	.server-title-none{
		width: 65px;
		background-color: 30px;
		background-color: #aaaaaa;
		font-size: 12px;
		color: #fff;
		line-height: 30px;
		text-align: center;
		border-radius: 30px;
		margin-left: 5px;
	}
	.details-bottom{
		width: 100%;
		height: 50px;
		background-color: #308bd1;
		position: fixed;
		bottom: 0px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		font-size: 15px;
		color: #fff;
	}
</style>
