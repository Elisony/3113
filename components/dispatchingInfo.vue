<template>
	<view class="list-address">
		<view class="list-item">
			<view class="item-left">
				<span class="left-spot">.</span>
				<span class="left-title">{{ item.take }}</span>
			</view>
			<view class="item-right">
			</view>
		</view>
		<view class="contacts-box">
			<view class="contacts-left">{{ item.take_name }}</view>
			<view class="contacts-center">{{ item.take_phone }}</view>
			<view class="contacts-right" v-show="item.take_phone !== ''">
				<view class="right-phone" @tap="phoneCall(item.take_phone)">
					<image src="/static/images/phone.png" mode=""></image>
				</view>
			</view>
		</view>
		<view class="list-item">
			<view class="item-down-box">
				<view class="item-down">
					<image src="/static/images/down.png" mode=""></image>
				</view>
			</view>
		</view>
		<view class="list-item">
			<view class="item-left">
				<span class="left-spot2">.</span>
				<span class="left-title">{{ item.receipt }}</span>
			</view>
			<view class="item-right">
			</view>
		</view>
		<view class="contacts-box">
			<view class="contacts-left">{{ item.receipt_name }}</view>
			<view class="contacts-center">{{ item.receipt_phone }}</view>
			<view class="contacts-right" v-show="item.receipt_phone !== ''">
				<view class="right-phone" @tap="phoneCall(item.receipt_phone)">
					<image src="/static/images/phone.png" mode=""></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: ['item'],
		data() {
			return {
				
			};
		},
		methods:{
			phoneCall(phone){
				uni.makePhoneCall({
				    phoneNumber: phone //仅为示例
				});
			}
		}
	}
</script>

<style>
	.list-address{
		width: 100%;
		height: 180px;
		border-bottom: 1px solid #eee;
		padding-top: 10px;
	}
	.list-item{
		width: 100%;
		height: 40px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}
	.item-left{
		width: 73%;
		height: 40px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: flex-start;
	}
	.item-right{
		width: 20%;
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
		justify-content: flex-start;
	}
	.contacts-box{
		width: 100%;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: space-around;
	}
	.contacts-left{
		width: 20%;
		display: flex;
		flex-direction: row;
		justify-content: flex-start;
		margin-left: 18px;
		font-size: 15px;
		color: #333;
		font-weight: 600;
	}
	.contacts-center{
		width: 30%;
		display: flex;
		flex-direction: row;
		justify-content: flex-start;
		font-size: 15px;
		color: #333;
		font-weight: 600;
	}
	.contacts-right{
		width: 26%;
		display: flex;
		flex-direction: row;
		justify-content: flex-end;
	}
	.right-phone{
		width: 23px;
		height: 23px;
	}
	.right-phone>image{
		width: 100%;
		height: 100%;
	}
</style>
