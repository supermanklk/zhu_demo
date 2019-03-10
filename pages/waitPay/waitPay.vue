<template>
	<view>
		<view class="waitPay">
			<image class="waitPay_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/money.png" mode=""></image>
			<text class="waitPay_text_1">待支付 <text style="font-size: 20px; display: inline-block;width: 100upx; " >699</text> 元</text>
			<text class="waitPay_text_2">选择合适的套餐、支付成功后开始审核材料</text>
		</view>
		
		<!-- 套餐选择 -->
		<view class="chooseApackage">
			<view :class="packageActiveIndex == 1 ? 'package package_active' : 'package' " data-index="1" @click="chooseApackage">
				<image class="package_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/taocan.png" mode=""></image>
				<text>套餐一</text>
			</view>
			<view :class="packageActiveIndex == 2 ? 'package package_active' : 'package' " data-index="2" @click="chooseApackage">
				<image class="package_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/taocan.png" mode=""></image>
				<text>套餐二</text>
			</view>
			<view :class="packageActiveIndex == 3 ? 'package package_active' : 'package' " data-index="3" @click="chooseApackage">
				<image class="package_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/taocan.png" mode=""></image>
				<text class="package_txt">套餐三</text>
			</view>
		</view>
		<!-- 套餐说明 -->
		<view class="package_description">
			<text>套餐{{packageActiveTxt}}说明:</text>
		</view>
		
		<!-- 套餐详情说明 -->
		<view class="package_package_description_detail">
			<div v-for="(item,index) in package" :key="index">
				<view class="">
					<text style="font-size: 14px;">{{ item }}</text>
				</view>
			</div>
		</view>
		
		<!-- 剩余支付时间 -->
		<view class="leave_time" ref="zhangbin">
			<text>剩余支付时间&nbsp;: &nbsp;&nbsp;29分59秒</text>
		</view>
		
		<!-- 确认支付按钮 -->
		<view class="pay_btn">
			<button @click="payMoney" type="" size="mini" plain="true" style="margin-top: 20px; width: 360upx; color: #1AAD19; border-color: #1AAD19;" hover-class = "btn_hover">
				确认支付
			</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				package : [
					'1、套餐一说明的第一条;',
					'2、套餐一说明的第二条;',
					'3、套餐一说明的第三条;',
					'4、套餐一说明的第四条;'
				],
				packageActiveIndex : 1,
				packageActiveTxt : '一'
			};
		},
		methods: {
			chooseApackage(e) {
				let index = e.currentTarget.dataset.index;
				this.packageActiveIndex = index;
				if(index == 1) {
					this.package = [
						'1、套餐一说明的第一条;',
						'2、套餐一说明的第二条;',
						'3、套餐一说明的第三条;',
						'4、套餐一说明的第四条;'
					];
					this.packageActiveTxt = '一';
				} else if(index == 2) {
					this.package = [
						'1、套餐二说明的第一条;',
						'2、套餐二说明的第二条;',
						'3、套餐二说明的第三条;',
						'4、套餐二说明的第四条;'
					];
					this.packageActiveTxt = '二';
				} else if (index == 3) {
					this.package = [
						'1、套餐三说明的第一条;',
						'2、套餐三说明的第二条;',
						'3、套餐三说明的第三条;',
						'4、套餐三说明的第四条;'
					];
					this.packageActiveTxt = '三';
				}
			},
			payMoney() {
				// 目前认为是支付成功
				// 支付成功要修改数据库 公司的
				try {
					const openid = uni.getStorageSync('openid');
					if (openid) {
							uni.request({
							url: global.host + 'Zhu/goToPay',
							method: 'GET',
							data: {
								openid : openid,
								is_pay : 1 // 1代表都成功了
							},
							success: res => {
								console.log('修改支付返回的数据',res);
								// 并且需要修改step
								try {
									const bussiness_name = uni.getStorageSync('bussiness_name');
									if (bussiness_name) {
										if (openid) {
												uni.request({
												url: global.host + 'Zhu/editCurrentStep',
												method: 'GET',
												data: {
													openid : openid,
													current_step : 4 // 4代表人工阶段
												},
												success: res => {
													// 提交核名
													uni.showLoading({
														title: '办理核名中...'
													});
													let S_OPSCOPE = encodeURIComponent('服装网店').replace(/25+/g,'');
													let S_DOM_LOCATION = encodeURIComponent('鑫景商城4栋71号444').replace(/25+/g,'');
													let S_FDDBR = encodeURIComponent('康乐').replace(/25+/g,'');
													let globalName = encodeURIComponent(bussiness_name.replace('名称查重通过', '')).replace(/25+/g,'');
													console.log(999)
													console.log(global.host + 'Zhu/sendName?S_OPSCOPE=' + S_OPSCOPE + '&S_DOM_LOCATION=' + S_DOM_LOCATION + '&S_FDDBR=' + S_FDDBR + '&globalName=' + globalName);
													console.log(888)
													uni.request({
														url: global.host + 'Zhu/sendName?S_OPSCOPE=' + S_OPSCOPE + '&S_DOM_LOCATION=' + S_DOM_LOCATION + '&S_FDDBR=' + S_FDDBR + '&globalName=' + globalName,
														method: 'GET',
														data: {
															S_MOBTEL : '18602197097',
														},
														success: res => {
															uni.hideLoading();
															console.log('提交查询',res);
															try {
																uni.setStorageSync('beian', res);
																	console.log('支付成功修改了step时候跳转',res);
																uni.redirectTo({
																	url: '../main_index/main_index?from=waitPay'
																});
															} catch (e) {
																// error
															}
														},
														fail: (e) => {
															console.log(e);
															uni.showToast({
																title: '已提交,请联系客服',
																duration: 2000
															});
															uni.request({
															url: global.host + 'Zhu/editCurrentStep',
															method: 'GET',
															data: {
																openid : openid,
																current_step : 3 
															},
															})
														},
														complete: () => {}
													});
													
												},
												fail: () => {},
												complete: () => {}
											});
										}
										
									}
								} catch (e) {
									// error
								}
								
							},
							fail: () => {},
							complete: () => {}
						});
					}
				} catch (e) {
					// error
					console.log('error888',e);
				}
			}
		}
	}
</script>

<style>
.waitPay {
	text-align: center;
	margin: 0 auto;
	display: flex;
	flex-direction: column;
	margin-top: 50px;
}
.waitPay_img {
	margin:  0 auto;
	width: 80upx;
	height: 40px;
}
.waitPay_text_1 {
	display: flex;
	align-items: center;
	margin-top: 15px;
	justify-content: center;
	font-size: 16px;
	color: #000000;
}
.waitPay_text_2 {
	margin-top: 10px;
	font-size: 14px;
	color: #000000;
}

.chooseApackage {
	margin: 0 auto;
	width: 690upx;
	height: 98px;
	background: #F5F5F5;
	margin-top: 20px;
	display: flex;
	flex-direction: row;
	justify-content:space-around;
	align-items: center;
}
.package {
	border-radius: 5px;
	width: 140upx;
	height: 74px;
	text-align: center;
	display: flex;
	flex-direction: column;
	justify-content: center;
}
.package_active {
	 background: white!important; 
}
.package_img {
	margin:  0 auto;
	width: 56upx;
	height: 28px;
	margin-bottom: 20upx;
}
.package_txt {
	text-align: center;
}
.package_description {
	font-weight: 600;
	margin-top: 25px;
	font-size: 14px;
	text-align: center;
}
.package_package_description_detail {
	margin-top: 15px;
	margin-left: 74upx;
	font-size: 14px;
}
.leave_time {
	text-align: center;
	margin: 0 auto;
	margin-top: 50px;
	font-size: 13px;
}
.pay_btn {
	margin: 0 auto;
	text-align: center;
}
</style>
