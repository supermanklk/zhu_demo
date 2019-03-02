<template>
	<view class="choiceInvitationCode">
		<input type="text"  value="1111"  ref="btn"/>
		<view class="choiceInvitationCode_title">
			<text>请输入邀请码</text>
		</view>
		<view class="choiceInvitationCode_header">
			<view  @tap="btns" class="choiceInvitationCode_header_item">
				
			</view>
			<view class="choiceInvitationCode_header_item">
				
			</view>
			<view class="choiceInvitationCode_header_item">
				
			</view>
			<view class="choiceInvitationCode_header_item">
				
			</view>
			<view class="choiceInvitationCode_header_item">
				
			</view>
			<view class="choiceInvitationCode_header_item">
				
			</view>
		</view>
		
		<view class="choiceInvitationCode_bottom">
			<image class="icon_img choiceInvitationCode_bottom_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/error_icon.png" mode=""></image>
			<text class="choiceInvitationCode_bottom_txt">邀请码无效</text>
		</view>
		
		<!-- 邀请成功的时候显示 -->
		<!-- <view class="choiceInvitationCode_bottom">
			<image class="icon_img choiceInvitationCode_bottom_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/success.png" mode=""></image>
			<text class="choiceInvitationCode_bottom_txt">验证通过</text>
		</view> -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
		onLoad() {
		},
		methods:{
			btns(){
				console.log(this.$refs.btn);
				// 获取元素的信息
// 				 uni.createSelectorQuery().select('.binbin').boundingClientRect(function(e){
//                 console.log(e)
//                 uni.pageScrollTo({
//                     scrollTop: e.top,
//                     duration: 800
//                 });
//             }).exec();

			}
		}
	}
</script>

<style>
.choiceInvitationCode {
	text-align: center;
}
.choiceInvitationCode_title {
	margin-top: 100px;
	text-align: center;
	margin-bottom: 70px;
}
.choiceInvitationCode_header  {
	text-align: center;
	/* margin-top: 100px; */
	display: flex;
	flex-direction: row;
	justify-content: space-around;
}
.choiceInvitationCode_header_item {
	border-radius: 5px;
	width: 100upx;
	height: 50px;
	line-height: 50px;
	border: 1px solid #E77D53;
	font-size: 25px;
	text-align: center;
}
.choiceInvitationCode_bottom {
	display: flex;
	flex-direction: column;
}
.choiceInvitationCode_bottom_img {
	margin: 0 auto;
	margin-top: 140px;
}
.choiceInvitationCode_bottom_txt {
	margin-top: 34px;
	font-size: 14px;
}
</style>