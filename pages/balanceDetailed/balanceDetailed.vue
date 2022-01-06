<template>
	<view class="content">
		<view class="top-back" @tap="navigateBack">
			<view class="top-left">
				<view class="back-box">
					<image src="/static/images/back.png" mode=""></image>
				</view>
			</view>
			<view class="top-right">
				余额变更明细
			</view>
		</view>
		<view class="detailed-info" v-show="show">
			<view class="detailed-search">
				<view class="search-left">
					选择周期
				</view>
				<view class="search-right">
					<view class="search-right-select">
						<picker @change="bindPickerChange" :value="index" :range="array">
							<view class="uni-input">{{array[index]}}</view>
						</picker>
					</view>
					<view class="search-right-title">
						<view class="search-right-more">
							<image src="/static/images/more-2.png" mode=""></image>
						</view>
					</view>
				</view>
			</view>
			<view class="search-item">
				<view class="search-item-left">
					<p class="search-left-line1">
						客户编号： 090939
					</p>
					<p class="search-left-line2">
						订单编号：43243242342424
					</p>
				</view>
				<view class="search-item-right">
					<p class="search-right-line1">
						+6.00元
					</p>
					<p class="search-right-line2">
						2018年6月21日17:29:30
					</p>
				</view>
			</view>
			<view class="search-item">
				<view class="search-item-left">
					<p class="search-left-line1">
						客户编号： 090939
					</p>
					<p class="search-left-line2">
						订单编号：43243242342424
					</p>
				</view>
				<view class="search-item-right">
					<p class="search-right-line1">
						+6.00元
					</p>
					<p class="search-right-line2">
						2018年6月21日17:29:30
					</p>
				</view>
			</view>
			<view class="search-item">
				<view class="search-item-left">
					<p class="search-left-line1">
						客户编号： 090939
					</p>
					<p class="search-left-line2">
						订单编号：43243242342424
					</p>
				</view>
				<view class="search-item-right">
					<p class="search-right-line1">
						+6.00元
					</p>
					<p class="search-right-line2">
						2018年6月21日17:29:30
					</p>
				</view>
			</view>
		</view>
		<view class="search-none" v-if="!show">
			近一个月内没有交易
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				array: ['最近一个月', '最近一周', '昨天', '今天'],
				index: 0,
				show: true
			}
		},
		methods: {
			bindPickerChange: function(e) {
				console.log('picker发送选择改变，携带值为', e.target.value)
				this.index = e.target.value
			},
			navigateBack(){
				uni.navigateBack()
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
		width: 36%;
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
	.detailed-info{
		width: 100%;
		height: 100%;
	}
	.detailed-search{
		width: 100%;
		height: 50px;
		background-color: #fff;
		border-bottom: 1px solid #eee;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		margin-bottom: 10px;
	}
	.search-left{
		width: 22%;
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
	.search-item{
		width: 100%;
		height: 90px;
		background-color: #fff;
		border-bottom: 1px solid #eee;
		margin-bottom: 10px;
		display: flex;
		flex-direction: row;
		justify-content: center;
	}
	.search-item-left{
		width: 46%;
		height: 90px;
	}
	.search-item-right{
		width: 46%;
		height: 90px;
		text-align: right;
	}
	.search-left-line1{
		font-size: 12px;
		color: #333;
		font-weight: 600;
		line-height: 40px;
		padding-top: 10px;
	}
	.search-left-line2{
		font-size: 12px;
		color: #ccc;
	}
	.search-right-line1{
		font-size: 12px;
		color: #333;
		font-weight: 600;
		line-height: 40px;
		padding-top: 10px;
	}
	.search-right-line2{
		font-size: 12px;
		color: #ccc;
	}
	.search-none{
		width: 100%;
		height: 50px;
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		font-size: 18px;
		color: #ccc;
		margin-top: 200px;
	}
</style>
