<!-- 名称查重通过页面 -->
<template>
	<view class="nameRepeatSuccess">
		<!-- icon 与 文字 -->
		<view class="nameRepeatSuccess_header">
			<image class="nameRepeatSuccess_header_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/name_repeat_ok.png" mode="" ></image>
			<text class="nameRepeatSuccess_header_txt">查重通过,名称可用</text>
			<!-- <text class="nameRepeatSuccess_header_txt">该名称将保留24小时,请尽快完成注册登记</text> -->
		</view>
		
		
		<view class="" style="width: 750upx; height: 2px; background: #E5E5E5;">
		</view>
		
		<view class="nameRepeatSuccess_prompt">
			<text>温馨提示</text>
		</view>
		
		<view class="nameRepeatSuccess_block">
			<text>1.查重结果仅供参考,须以最后审核结果为准</text>
			<text>2.点击确认后请在24小时内完成注册登记</text>
		</view>
		
		<!-- 确认支付按钮 -->
		<view class="goTorRegistration">
			<button @click="goTorRegistration" type="" size="mini" plain="true" style="margin-top: 20px; width: 360upx; color: #1AAD19; border-color: #1AAD19;" hover-class = "btn_hover">
				确认
			</button>
		</view>
		
		<view class="">
			<text>重新查询</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
		methods:{
			goTorRegistration() {
				// 查重成功,去注册登记页面
// 				uni.navigateTo({
// 					url: '../registration/registration'
// 				});
				uni.reLaunch({
					url: '../main_index/main_index?from=name_repeat'
				});
			},
			clickOk() {
				
			}
		}
	}
</script>

<style>
.nameRepeatSuccess_prompt {
	font-size: 14px;
	margin-top: 25px;
}
.nameRepeatSuccess_block {
	text-align: left;
	box-sizing: border-box;
	width: 690upx;
	height: 70px;
	display: flex;
	flex-direction: column;
	padding-left: 30px;
	padding-top: 10px;
	padding-bottom: 10px;
	font-size: 14px;
	background: #F5F5F5;
	margin: 0 auto;
	margin-top: 10px;

}
.nameRepeatSuccess {
	text-align: center;
	margin-top: 50px;
	/* display: flex;
	flex-direction: column; */
}
.nameRepeatSuccess_header {
	display: flex;
	flex-direction: column;
}
.nameRepeatSuccess_header_img {
	margin: 0 auto;
	width: 80upx;
	height: 40px;
}
.nameRepeatSuccess_header_txt {
	margin-top: 34px;
	font-size: 14px;
	margin-bottom: 25px;
}
.choiceIndustry_pre_nex {
	/* margin-top: 350px; */
	margin-bottom: 55px;
	display: flex;
	flex-direction: row;
	/* justify-content: space-around; */
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
 
 .goTorRegistration {
	 margin: 0 auto;
	 margin-top: 200px;
	 text-align: center;
 }
</style>
