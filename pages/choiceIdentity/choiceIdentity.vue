<!-- 选择身份 -->
<template>
	<view class="choiceIdentity">
		<view class="choiceIdentity_header">
			<text>选择你的身份</text>
		</view>
		
		<view :class="activeIndex == 1 ? 'choiceIdentity_block_1 active' : 'choiceIdentity_block_1' " data-index="1" @click="choiceIdentity">
			<view class="">
				<text>我是普通个人卖家</text>
			</view>
			<view class="">
				<text>(淘宝 京东 拼多多 微商等个人卖家)</text>
			</view>
		</view>
		
		<view :class="activeIndex == 2 ? 'choiceIdentity_block_2 active' : 'choiceIdentity_block_2'" data-index="2"  @click="choiceIdentity">
			<text>我有邀请码</text>
		</view>
		
		<view class="choiceIdentity_bottom" @click="clickNext">
			<image class="choiceIdentity_bottom_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/right_icon.png" mode=""></image>
			<text class="choiceIdentity_bottom_txt">下一步</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				activeIndex  : 1
			};
		},
		methods: {
			choiceIdentity(e) {
				if(e.currentTarget.dataset.index == "1") {
					this.activeIndex = 1;
				} else {
					this.activeIndex = 2;
				}
			},
			clickNext() {
				if(this.activeIndex == 1) {
					// 往普通卖家跳转
					// 同时需要修改进度,选择身份阶段已经过
					try {
						const openid = uni.getStorageSync('openid');
						if (openid) {
								uni.request({
								url: global.host + 'Zhu/editCurrentStep',
								method: 'GET',
								data: {
									openid : openid,
									current_step : 0 // 0代表处理选择行业阶段
								},
								success: res => {
									console.log('选择身份后,进行跳转main之前,修改状态',res);
									// 修改数据库身份
									uni.request({
										url: global.host + '/Zhu/changeUserType',
										method: 'GET',
										data: {
											openid : openid,
											user_type : 1 //普通卖家
										},
										success: res => {
											console.log('修改用户身份之后跳转',res);
											uni.reLaunch({
												url: '../main_index/main_index'
											});
										},
										fail: () => {},
										complete: () => {}
									});
								},
								fail: () => {},
								complete: () => {}
							});
						}
					} catch (e) {
						// error
						console.log('error888',e);
					}
					
				} else {
					// 往有邀请码的卖家跳转
					uni.navigateTo({
						url: '../choiceInvitationCode/choiceInvitationCode'
					});
				}
				
			}
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
	font-size: 20px;
	margin-top: 100px;
}

.choiceIdentity_block_1 {
	margin: 0 auto;
	margin-top: 68px;
	padding-top: 10px;
	width: 603upx;
	height: 65px;
	background: white;
	
	color: #E77D53;
	border: 1px solid #E77D53;
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

.active {
	background: #E77D53!important; 
	color: white!important;
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
