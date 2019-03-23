<template>
	<view class="choiceInvitationCode">
		<!-- 这个input就是验证码的点睛之笔 -->
		<input type="number"  style="display: none;" :focus="focus" :value="inputValue"  @input="changeNumber" ref="btn" id="btn"/>
		<view class="choiceInvitationCode_title">
			<text>请输入邀请码</text>
		</view>
		<view class="choiceInvitationCode_header">
			<view  @tap="btns" class="choiceInvitationCode_header_item">
				<text>{{number[0]}}</text>
			</view>
			<view @tap="btns" class="choiceInvitationCode_header_item">
				<text>{{number[1]}}</text>
			</view>
			<view @tap="btns" class="choiceInvitationCode_header_item">
				<text>{{number[2]}}</text>
			</view>
			<view @tap="btns" class="choiceInvitationCode_header_item">
				<text>{{number[3]}}</text>
			</view>
			<view @tap="btns" class="choiceInvitationCode_header_item">
				<text>{{number[4]}}</text>
			</view>
			<view @tap="btns" class="choiceInvitationCode_header_item">
				<text>{{number[5]}}</text>
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
				focus : false ,// 控制数字键盘的显隐
				number : [],
				inputValue : ""
			};
		},
		onLoad() {
		},
		methods:{
			btns(){
				this.focus = !this.focus;
				// console.log(this.$refs.btn);
				// 获取元素的信息
// 				 let view = uni.createSelectorQuery().select('#btn').boundingClientRect(function(e){
//                 console.log(e)
//                 uni.pageScrollTo({
//                     scrollTop: e.top,
//                     duration: 800
//                 });
//             }).exec();

			// let view = uni.createSelectorQuery().select("#btn");


			},
			changeNumber(e) {				
				if(e.detail.value.length >7 ) {
					return false
				} else {
					if(e.detail.value.length == 6) {
						this.inputValue = e.detail.value;
						this.number = e.detail.value.split((''));
						this.focus = false;
						// 这里就是输入完验证码,需要开始调用校验邀请码的的函数
						if(e.detail.value == '724724' ||e.detail.value == '334455') {
							let arr = ['健康销售'];
							uni.setStorageSync('business_scope', arr);
							uni.setStorageSync('innercode', e.detail.value);
							const openid = uni.getStorageSync('openid');
							if (openid) {
									uni.request({
									url: global.host + 'Zhu/editCurrentStep',
									method: 'GET',
									data: {
										openid : openid,
										current_step : 1 // 0代表处理选择行业阶段
									},
									success: res => {
										uni.showToast({
											title: '通过邀请,正在跳转',
											duration: 3000,
											success : function(){
												uni.reLaunch({
													url: '../main_index/main_index?from=choiceInvitationCode'
												});
											}
										});
									},
									fail: () => {},
									complete: () => {}
								});
							}
							
						} else {
							
						}
						
					}else {
						console.log(2);
						this.inputValue = e.detail.value;
						this.number = e.detail.value.split((''));
					}
					
				}
				
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