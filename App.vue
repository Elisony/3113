<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
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
	  font-family: 'iconfont';  /* Project id 2979078 */
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
