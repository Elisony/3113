<template>
	<view class="content">
		<view class="top-back">
			<view class="top-left">
				<navigator url="/pages/orderTaking/orderTaking" hover-class="navigator-hover" class="back-box">
					<image src="/static/images/back.png" mode=""></image>
				</navigator>
			</view>
			<view class="top-right">
				选择取消订单原因
			</view>
		</view>
		<view class="cancel-info">
			<view class="cancel-info-item" v-for="(item, index) in cancelList" :key="index">
				<view class="cancel-item-left">
					1、{{ item.name }}
				</view>
				<view class="cancel-item-right">
					 <radio-group @change="radioChange">
						<label class="uni-list-cell uni-list-cell-pd">
							<view>
								<radio :value="item.name" :checked="index === current" color="#308bd1" style="transform: scale(0.6); color: #308bd1;"/>
							</view>
						</label>
					</radio-group>
				</view>
			</view>
		</view>
		<view class="cancel-bottom" @click="nextTick">
			下一步
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				current: 0,
				order_id: 0,
				cancelList: [],
				cancel_msg: ""
			}
		},
		onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
			
			let then = this
			then.order_id = option.id
			
			//获取取消订单选项列表	
			then.getOrderCancelList()
		},
		methods: {
			radioChange: function(evt) {
				for (let i = 0; i < this.cancelList.length; i++) {
					if (this.cancelList[i].name === evt.detail.value) {
						this.current = i;
						break;
					}
				}
				this.cancel_msg = evt.detail.value
			},
			
			//获取取消订单选项列表
			getOrderCancelList(){
				uni.request({
					url: "http://test.qd-happy.com/app_service",
					method: "POST",
					header: {
						'Content-Type': "multipart/form-data",
					},
					data: {
						interface: "order_cancelList",
						data: {
							
						}
					},
					success: (res) => {
						this.cancelList = res.data.data.list
					}
				})
			},
			
			//下一步
			nextTick(){
				if (!this.cancel_msg) {
					this.cancel_msg = this.cancelList[this.current].name
				}
				uni.navigateTo({
					url: '/pages/cancelReason/cancelReason?order_id='+this.order_id+'&cancel_msg='+this.cancel_msg
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
		position: relative;
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
		width: 32%;
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
	.cancel-info{
		width: 100%;
		height: 100%;
	}
	.cancel-info-item{
		width: 100%;
		height: 50px;
		background-color: #fff;
		border-bottom: 1px solid #ccc;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		margin-bottom: 10px;
	}
	.cancel-item-left{
		width: 50%;
		height: 50px;
		line-height: 50px;
		text-align: left;
		padding-left: 10px;
		font-size: 18px;
		color: #333;
	}
	.cancel-item-right{
		width: 50%;
		height: 50px;
		line-height: 50px;
		text-align: right;
		padding-right: 10px;
	}
	.cancel-bottom{
		width: 100%;
		height: 50px;
		position: fixed;
		bottom: 0px;
		background-color: #308bd1;
		font-size: 15px;
		color: #fff;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}
</style>
