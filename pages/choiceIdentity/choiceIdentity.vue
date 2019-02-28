<!-- 选择身份 -->
<template>
	<view class="choiceIdentity">
		<view class="choiceIdentity_header">
			<text>选择你的身份</text>
		</view>
		
		<view class="choiceIdentity_block_1">
			<view class="">
				<text>我是普通个人卖家</text>
			</view>
			<view class="">
				<text>(淘宝 京东 拼多多 微商等个人卖家)</text>
			</view>
		</view>
		
		<view class="choiceIdentity_block_2">
			<text>我有邀请码</text>
		</view>
		
		<view class="choiceIdentity_bottom">
			<image class="choiceIdentity_bottom_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/right_icon.png" mode=""></image>
			<text class="choiceIdentity_bottom_txt">下一步</text>
		</view>
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
.choiceIdentity {
	text-align: center;
	font-size: 17px;
	font-style: normal;
}
.choiceIdentity_header {
	margin-top: 100px;
}

.choiceIdentity_block_1 {
	margin: 0 auto;
	margin-top: 68px;
	padding-top: 10px;
	width: 603upx;
	height: 65px;
	background: #E77D53;
	color: white;
	font-size: 15px;
	border-radius: 5px;
}

.choiceIdentity_block_2 {
	color: #E77D53;
	font-size: 15px;
	margin: 0 auto;
	margin-top: 25px;
	width: 603upx;
	height: 65px;
	background: white;
	border: 1px solid #E77D53;
	border-radius: 5px;
	box-sizing: border-box;
	padding-top: 18px;
}

.choiceIdentity_bottom {
	margin-top: 150px;
	display: flex;
	flex-direction: column;
	justify-content: center;
}

.choiceIdentity_bottom_img {
	margin: 0 auto;
	width: 40upx;
	height: 20px;
}

.choiceIdentity_bottom_txt {
	font-size: 14px;
	margin-top: 10px;
}
</style>
