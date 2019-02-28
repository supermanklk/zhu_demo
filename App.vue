<script>
	export default {
		onLaunch: function () {
			console.log('App Launch')
		},
		onShow: function () {
			console.log('App Show')
		},
		onHide: function () {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
	/* uni.css - 通用组件、模板样式库，可以当作一套ui库应用 */
	@import './common/uni.css';
	.red {
		color:  yellow;
	}
	.gray {
		color:  gray;
	}
	.choiceIndustry_pre_nex {
		margin-top: 350px;
		margin-bottom: 55px;
		display: flex;
		flex-direction: row;
		justify-content: space-around;
	}
	.choiceIndustry_pre_nex_left {
		display: flex;
		flex-direction: column;
	}
	
	.choiceIndustry_pre_nex_right {
		display: flex;
		flex-direction: column;
		
		/* 灰色 */
		/* color: #dbdbdb; */
	}
	
	.choiceIndustry_pre_nex_left_img {
		margin: 0 auto;
		width: 40upx;
		height: 20px;
		margin-bottom: 10px;
	}
	
	 .choiceIndustry_pre_nex_right_img {
		margin: 0 auto;
		width: 40upx;
		height: 20px;
		margin-bottom: 10px;
	 }
</style>
