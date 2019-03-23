<!-- 名称查重失败 和 提交超时 -->
<template>
	<view class="name_repeat_error">
		<!-- 查看错误弹窗 -->
		<view :class="alertModal == true ? 'name_repeat_error_modal' : 'no_name_repeat_error_modal' ">
			<view class="name_repeat_error_title">
				<text>失败原因</text>
			</view>
			<view class="name_repeat_error_list">
				<text>1、{{errormsg1}}</text>
				<!-- <text>2、失败原因二</text>
				<text>3、失败原因三</text>
				<text>4、失败原因四</text> -->
			</view>
		</view>
		<!-- 弹窗关闭的X -->
		<view :class="alertModal == true ? 'name_repeat_error_modal_X' : 'no_name_repeat_error_modal_X' ">
			<image class="alert_img_x_item" @click="closeImgAlert" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/error.png" mode="" ></image>
		</view>
		<!-- 蒙层 -->
		<view :class="alertModal == true ? 'mengceng' : ''">
		</view>
		
		
		<view class="name_repeat_error_header">
			<image class="name_repeat_error_header_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/error_icon.png" mode=""></image>
			<view class="name_repeat_error_header_txt">
				<text style="font-size: 14px;">查重失败,</text>
				<text class="find_idea" @click="click_error"> &nbsp;&nbsp;点击查看原因</text>
			</view>
		</view>
		
		<!-- 推荐列表 -->
		<view class="name_repeat_list">
			<view class="" v-for="(item,index) in list" :key="index" @click="goToBack">
				<view class="list_item">
					{{item.name}}
				</view>
			</view>
		</view>
		
		<!-- 换一批 -->
		<view class="changeBatch">
			<text class="changeBatch_txt">换一批</text>
			<image class="changeBatch_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/changeBatch.png"></image>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				alertModal : false, // 查看失败原因弹窗
				list : [
					{ name : '推荐名称一'},
					{ name : '推荐名称二'},
					{ name : '推荐名称三'},
					{ name : '推荐名称四'},
					{ name : '推荐名称五'}
				],
				errormsg1 : '',
			};
		},
		onLoad(e) {
			console.log('进到了错误页面',e);
			if(e) {
				if(e.errormsg.indexOf('timed')!= -1) {
					this.errormsg1 = '系统错误,请重试';
				} else {
					this.errormsg1 = '系统错误,请重试.错误码:400312';
					// this.errormsg1 = e.errormsg;
				}
			}
		},
		methods: {
			// uni-app的弹窗组件都是这个样子的
			click_error(e) {
				this.alertModal = true;
			},
			closeImgAlert() {
				this.alertModal = false;
			},
			goToBack() {
				uni.redirectTo({
					url: '../name_repeat/name_repeat'
				});
			}
		}
	}
</script>

<style>
/* 弹窗样式 */
.name_repeat_error_modal {
	height: 310px;
	width: 570upx;
	background-color: white;
	z-index: 999;
	position: fixed;
	top: 125px;
	left: 45px;
	border-radius: 5px;
}
.no_name_repeat_error_modal {
	display: none;
}
/* 关闭的X */
.name_repeat_error_modal_X {
	width: 60upx;
	height: 30px;
	position: fixed;
	z-index: 999;
	top: 450px;
	left: 172px;
}
.no_name_repeat_error_modal_X {
	display: none;
}
/* 蒙层 */
.mengceng {
	background-color:rgba(151,151,151,0.8);
	width: 750upx;
	height: 100%;
	position: fixed;
	top: 0;
	z-index: 998;
}
.alert_img_x_item {
	width: 60upx;
	height: 30px;
	position: fixed;
	top: 450px;
	left: 330upx;
	z-index: 999;
}
	
.name_repeat_error_title {
	margin-top: 10px;
	font-size: 15px;
	text-align: center;
}
.name_repeat_error_list {
	margin-top: 10px;
	display: flex;
	flex-direction: column;
	margin-left: 19px;
	margin-right: 19px;
}

	
.name_repeat_error {
	
}
.name_repeat_error_header {
	margin-top: 50px;
	width: 750upx;
	display: flex;
	flex-direction: column;
	align-items: center;
}
.name_repeat_error_header_img {
	width: 80upx;
	height: 40px;
	margin-bottom: 34px;
}
.name_repeat_error_header_txt {
	font-size: 14px;
	font-style: normal;
}
.find_idea { 
	color: #BD10E0;
	font-size: 12px;
}
.name_repeat_list {
	position: relative;
	border-radius: 5px;
	border: 1px solid #E5E5E5;
	margin: 0 auto;
	margin-top: 70px;
	width: 700upx;
}
.list_item {
	font-size: 14px;
	/* padding: 10px auto 10px 16px; */
	padding-top: 10px;
	padding-bottom: 10px;
	padding-left: 16px;
	text-align: left;
	border-bottom: 1px solid #E5E5E5;
}

.changeBatch {
	margin-top: 20px;
	justify-content: center;
	display: flex;
	flex-direction: row;
	color: #E77D53;
}
.changeBatch_txt {
	font-style: normal;
	font-size: 14px;
}
.changeBatch_img {
	margin-left: 10upx;
	width: 40upx;
	height: 20px;
}
</style>
