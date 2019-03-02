<!-- 名称查重通过页面 -->
<template>
	<view class="nameRepeatSuccess">
		<!-- icon 与 文字 -->
		<view class="nameRepeatSuccess_header">
			<image class="nameRepeatSuccess_header_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/name_repeat_ok.png" mode="" ></image>
			<text class="nameRepeatSuccess_header_txt">查重通过,名称可用</text>
			<text class="nameRepeatSuccess_header_txt">该名称将保留24小时,请尽快完成注册登记</text>
		</view>
		
		<!-- 底部重新查询 注册登记 -->
		<view class="choiceIndustry_pre_nex">
			<view class="choiceIndustry_pre_nex_left">
				<image class="choiceIndustry_pre_nex_left_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/left_right.png" mode=""></image>
				<text class="choiceIndustry_pre_nex_left_txt">重新查询</text>
			</view>
			<view class="choiceIndustry_pre_nex_right" @click="goTorRegistration">
				<image class="choiceIndustry_pre_nex_right_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/right_icon.png" mode=""></image>
				<text class="choiceIndustry_pre_nex_right_txt">注册登记</text>
			</view>
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
			}
		}
	}
</script>

<style>
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
