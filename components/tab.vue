<template>
	<view class="tab-nav">
		<view class="tab-left">
			<view class="left-refresh">
				<image src="/static/images/refresh.png" mode=""></image>
			</view>
			<span class="refresh-title">
				刷新列表
			</span>
		</view>
		<navigator url="/pages/myTask/myTask" hover-class="navigator-hover" class="tab-right">
			<view class="right-task">
				<image src="/static/images/task.png" mode=""></image>
			</view>
			<p class="right-title">我的任务</p>
		</navigator>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		}
	}
</script>

<style>
	.tab-nav{
		width: 100%;
		height: 45px;
		background-color: #ccc;
		position: fixed;
		bottom: 0px;
		display: flex;
		flex-direction: row;
		align-items: center;
	}
	.tab-left{
		width: 80%;
		height: 45px;
		background-color: #308bd1;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}
	.tab-right{
		width: 20%;
		height: 45px;
		background-color: #fff;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
	.left-refresh{
		width: 21px;
		height: 17px;
	}
	.left-refresh>image{
		width: 100%;
		height: 100%;
	}
	.refresh-title{
		font-size: 15px;
		color: #fff;
		padding-left: 10px;
	}
	.right-task{
		width: 21px;
		height: 21px;
	}
	.right-task>image{
		width: 100%;
		height: 100%;
	}
	.right-title{
		font-size: 10px;
		color: #333333;
	}
</style>
