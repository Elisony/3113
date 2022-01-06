<template>
	<view class="alert-box" :style="{height: QHeight+'px'}"  v-show="show">
		<view class="alert-info">
			<view class="alert-info-title">
				{{ alertData.title }}
			</view>
			<view class="alert-info-content">
				{{ alertData.content }}
			</view>
			<bitton class="alert-info-button" @click="colose">{{ alertData.button }}</bitton>
		</view>
	</view>
</template>

<script>
	export default {
		props:['QHeight', 'alertData', 'show'],
		data() {
			return {
				
			}
		},
		methods:{
			colose(){
				this.show = false
			}
		}
	}
</script>

<style>
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
</style>
