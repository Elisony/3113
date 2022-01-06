<template>
	<view class="content">
		<image :style="{height: QHeight+'px'}" class="logo" src="/static/start_screen.jpg"></image>
	</view>
</template>

<script>
	var time = ''
	export default {
		data() {
			return {
				QHeight: 0,
				time: 5
			}
		},
		onLoad() {
			let then = this
			//获取屏幕高度
			uni.getSystemInfo({
				success: (res) => {
					let height = res.windowHeight - uni.upx2px(0)
					console.log(height)
					then.QHeight = height
				}
			})
			
			let timeS = then.time
			this.QTime(timeS)
		},
		methods: {
			QTime(timeS){
				let timeStrat = setInterval(function(){
					timeS--
					if (timeS == 0) {
						clearInterval(timeStrat)
						uni.redirectTo({
							url: "/pages/singIn/singIn"
						})
					}
					console.log(timeS)
				}, 1000, timeS)
				
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
	}

	.logo {
		width: 100%;
		
	}

	.text-area {
		display: flex;
		justify-content: center;
	}
</style>
