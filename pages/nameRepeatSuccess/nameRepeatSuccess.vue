<!-- 名称查重通过页面 -->
<template>
	<view class="nameRepeatSuccess">
		<!-- icon 与 文字 -->
		<view class="nameRepeatSuccess_header">
			<image class="nameRepeatSuccess_header_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/name_repeat_ok.png" mode="" ></image>
			<text class="nameRepeatSuccess_header_txt">查重通过</text>
			<!-- <text class="nameRepeatSuccess_header_txt">该名称将保留24小时,请尽快完成注册登记</text> -->
		</view>
		
		
		<view class="" style="width: 750upx; height: 1px; background: #E5E5E5;">
		</view>
		
		<view class="nameRepeatSuccess_prompt">
			<text>温馨提示</text>
		</view>
		
		<view class="nameRepeatSuccess_block">
			<text>1、查重结果仅供参考，须以最后审核结果为准</text>
			<text>2、请在24小时内完成注册登记</text>
		</view>
		
		<!-- 确认支付按钮 -->
		<view class="goTorRegistration">
			<button @click="goTorRegistration" type="" size="mini" plain="true" style="margin-top: 20px; width: 360upx; color: #1AAD19; border-color: #1AAD19;" hover-class = "btn_hover">
				确认
			</button>
		</view>
		
		<view class="" @click="repeatFind" style="height: 30px;">
			<text>重新查询</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				zi_hao : '',
				industry_description : '',
				organization_form : ''
			};
		},
		onLoad(e) {
			console.log(e);
			if(e) {
				this.zi_hao = decodeURIComponent(e.name);
				this.industry_description = decodeURIComponent(e.business);
				this.organization_form = e.organization;
			}
			
		},
		methods:{
			// 重新查重
			repeatFind() {
				uni.redirectTo({
					url: '../name_repeat/name_repeat'
				});
			},
			goTorRegistration() {
				// 查重成功,去注册登记页面
				// 点击确定,保留执照name 并切改step
				try {
					const bussiness_name = uni.getStorageSync('bussiness_name');
					if (bussiness_name) {
						// 保留name,修改step
						try {
							const business_scope = uni.getStorageSync('business_scope');
							const openid = uni.getStorageSync('openid');
							if (business_scope && openid) {
								uni.request({
									url: global.host + 'Zhu/setCompanyInfo',
									method: 'GET',
									data: {
										openid : openid,
										business_scope : business_scope,
										zi_hao : this.zi_hao,
										organization_form : this.organization_form,
										industry_description : this.industry_description,
										name_check : 1 // 代表通过
									},
									success: res => {
											// 保存成功修改step到注册登记
											try {
												const openid = uni.getStorageSync('openid');
												if (openid) {
														uni.request({
														url: global.host + 'Zhu/editCurrentStep',
														method: 'GET',
														data: {
															openid : openid,
															current_step : 2 // 2代表注册登记阶段
														},
														success: res => {
															uni.reLaunch({
																url: '../main_index/main_index?from=name_repeat'
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
									},
									fail: (e) => {
										console.log('查重报错',e);
									},
									complete: () => {}
								});
							}
							
						} catch (e) {
							// error
						}
						
					}
				} catch (e) {
					// error
				}
				
			},
			clickOk() {
				
			}
		}
	}
</script>

<style>
.nameRepeatSuccess_prompt {
	font-family: PingFangSC-Light;
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
	width: 120upx;
	height: 60px;
}
.nameRepeatSuccess_header_txt {
	margin-top: 34px;
	font-size: 20px;
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
	 margin-top: 165px;
	 margin-bottom: 15px;
	 text-align: center;
 }
</style>
