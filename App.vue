<script>
	export default {
		onLaunch: function() {
			this.update()
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		},
		methods: {
			update() {
				var _this = this;
				uni.request({
					url: `${this.$store.state.apiBaseUrl}/users/versions`, //请求接口
					method: 'POST',
					success: result => {
						if (result.data.code == 1) {
			 			plus.runtime.getProperty(plus.runtime.appid, function(inf) {
								if (inf.version != result.data.data.versions) {
									uni.showModal({
										title: "发现新版本",
										content: "确认下载更新",
										success: (res) => {
											if (res.confirm == true) { //当用户确定更新，执行更新
												_this.doUpData();
											}
										}
									})
								}
							});
						}
					},
				})
			},

			doUpData() {
				uni.showLoading({
					title: '更新中……'
				})
				uni.downloadFile({ //执行下载
					url: '***', //下载地址
					success: downloadResult => { //下载成功
						uni.hideLoading();
						if (downloadResult.statusCode == 200) {
							uni.showModal({
								title: '',
								content: '更新成功，确定现在重启吗？',
								confirmText: '重启',
								confirmColor: '#EE8F57',
								success: function(res) {
									if (res.confirm == true) {
										plus.runtime.install( //安装
											downloadResult.tempFilePath, {
												force: true
											},
											function(res) {
												utils.showToast('更新成功，重启中');
												plus.runtime.restart();
											}
										);
									}
								}
							});
						}
					}
				});
			}
		}
	}
</script>

<style>
	/*每个页面公共css */
	@font-face {
		font-family: test1-icon;
		src: url('~@/static/iconfont/PingFangSCRegular.ttf');
	}

	@font-face {
		font-family: 'iconfont';
		/* Project id 2979078 */
		src: url('https://at.alicdn.com/t/font_2979078_cay4xu2hw4f.woff2?t=1638331821562') format('woff2'),
			url('https://at.alicdn.com/t/font_2979078_cay4xu2hw4f.woff?t=1638331821562') format('woff'),
			url('https://at.alicdn.com/t/font_2979078_cay4xu2hw4f.ttf?t=1638331821562') format('truetype');
	}


	.iconfont {
		font-family: "iconfont" !important;
		font-size: 16px;
		font-style: normal;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
	}

	.icon-fanhui:before {
		content: "\e600";
	}
</style>
