<template>
	<view class="list-address">
		<view class="list-item">
			<view class="item-left">
				<span class="left-spot">.</span>
				<span class="left-title">{{ item.take }}</span>
			</view>
			<view class="item-right">
				{{ item.take_len }}
			</view>
		</view>
		<view class="list-item">
			<view class="item-down-box">
				<view class="item-down">
					<image src="/static/images/down.png" mode=""></image>
				</view>
				<span class="down-title">定福装西街和残街交汇处南行10米</span>
			</view>
		</view>
		<view class="list-item">
			<view class="item-left">
				<span class="left-spot2">.</span>
				<span class="left-title">{{ item.receipt }}</span>
			</view>
			<view class="item-right">
				{{ item.receipt_len }}
			</view>
		</view>
		<view class="contacts-box">
			<view class="contacts-left">王先生</view>
			<view class="contacts-center">13482334527</view>
			<view class="contacts-right">
				<view class="right-phone">
					<image src="/static/images/phone.png" mode=""></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props:['item'],
		data() {
			return {
				
			};
		}
	}
</script>

<style>
	.list-address{
		width: 100%;
		height: auto;
		border-bottom: 1px solid #eee;
	}
	.list-item{
		width: 100%;
		height: 30px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}
	.item-left{
		width: 70%;
		height: 40px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: flex-start;
	}
	.item-right{
		width: 22%;
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
	}
	.item-down-box{
		width: 92%;
		height: 30px;
		display: flex;
		flex-direction: row;
		align-items: center;
	}
	.item-down{
		width: 8px;
		height: 14px;
	}
	.item-down>image{
		width: 100%;
		height: 100%;
	}
	.down-title{
		font-size: 12px;
		color: #ccc;
		padding-left: 10px;
	}
	.left-spot2{
		font-size: 30px;
		color: red;
		padding-bottom: 20px;
	}
	.contacts-box{
		width: 100%;
		height: 35px;
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
