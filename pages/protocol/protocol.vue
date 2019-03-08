<template>
	<view class="protocol">
		<view class="protocol_title">
			<text>筑商平台个体户注册综合服务协议</text>
		</view>
		<view class="protocol_title2">
			<text>合同编号:</text><text style="margin-left: 3px; color: #FF0000; font-size: 13px;">系统自动生成</text>
		</view>
		<view class="protocol_block">
			<view class="protocol_block_1">
				欢迎您（或称“甲方”）与筑商平台（包括XX等网站及APP、微信公众号等客户端）的经营主体筑商公司（即浙江景宁筑商信息科技有限公司，或称“乙方”）共同签署本《筑商平台个体户注册综合服务协议》（下称“本协议”），委托筑商公司为您提供快速、便捷的经营所需的基本服务支持，包括但不限于个体工商户在线营业执照申办、入驻指定的孵化器、孵化管理及财税代理服务！
			</view>
			<view class="protocol_block_2">
				签署声明：当您按照页面提示阅读并同意本协议时，或您以其他筑商平台允许的方式实际使用本协议项下服务时，即表示您已充分阅读、理解并接受本协议的全部内容，并与筑商公司达成本协议。您承诺接受并遵守本协议的所有约定，您不应以未阅读本协议的内容或未获得筑商平台对您问询的解答等理由，主张本协议无效，或要求撤销本协议。筑商公司有权随时变更本协议并在筑商平台
			</view>
			
		</view>
		
		<view class="protocol_bottom" @click="goBack">
			<image  class="protocol_pre_nex_left_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/left_right.png" mode=""></image>
			<text>返回</text>
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
			goBack() {
				// 返回上一步
				uni.navigateBack({
					delta: 1
				});
			}
		}
	}
</script>

<style>
.protocol {
	text-align: center;
}
.protocol_title {
	margin-top: 25px;
	font-size: 17px;
}
.protocol_title2 {
	margin-top: 10px;
	font-size: 14px;
}
.protocol_block {
	box-sizing: border-box;
	border-radius: 5px;
	padding: 5px;
	border: 1px solid #D8D8D8;
	padding-left: 10px;
	padding-right: 10px;
	text-indent: 30px;
	text-align: left;
	width: 700upx;
	margin: 0 auto;
	margin-top: 10px;
	
}
.protocol_block_1 {
	font-size: 13px!important;
	text-indent: 30px;
}
.protocol_block_2 {
	font-size: 13px!important;
	margin-top: 20px;
	text-indent: 30px;
}
.br {
	height: 10px;
}
.protocol_pre_nex_left_img {
	margin: 0 auto;
	width: 40upx;
	height: 20px;
	margin-bottom: 10px;
}
.protocol_bottom {
	margin-top: 50px;
	display: flex;
	flex-direction: column;
	text-align: center;
}
</style>
