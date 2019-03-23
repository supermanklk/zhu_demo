<template>
	<view>
		<view class="waitPay">
			<image class="waitPay_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/money.png" mode=""></image>
			<text class="waitPay_text_1" style="font-size: 12px;">待支付 <text style="font-size: 20px; display: inline-block;width: 100upx; padding-left: 10px; padding-right: 12px;" >￥{{money}}</text> 原价￥1999</text> 
			<image style="position: absolute; top: 100px; right: 180upx; width: 97upx; height: 15px;" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/tehui.png" mode=""></image>
			<text class="waitPay_text_2">选择合适的套餐、支付成功后开始审核材料</text>
		</view>
		
		<!-- 套餐选择 -->
		<view class="chooseApackage">
			<!-- <view :class="packageActiveIndex == 1 ? 'package package_active' : 'package' " data-index="1" @click="chooseApackage"> -->
			<view :class="packageActiveIndex == 1 ? 'package package_active' : 'package' " data-index="1" @click="">
				<image class="package_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/taocan.png" mode=""></image>
				<text>套餐一</text>
			</view>
			<view :class="packageActiveIndex == 2 ? 'package package_active' : 'package' " data-index="2" @click="">
				<image class="package_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/taocan.png" mode=""></image>
				<text style="font-size: 9px;">套餐二(尽请期待)</text>
			</view>
			<view :class="packageActiveIndex == 3 ? 'package package_active' : 'package' " data-index="3" @click="">
				<image class="package_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/taocan.png" mode=""></image>
				<text style="font-size: 9px;">套餐三(尽请期待)</text>
				<!-- <text style="font-size: 8px;">尽请期待</text> -->
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
	var _self,user_name,user_phone,order_no,typeuser = '普通';
	var account = 'supermanzhangbin', password = 'faith1314z', aisle = '1'; // aisle 为通道 默认为1 就是没有更多按钮 
	export default {
		data() {
			return {
				package : [
					'1、注册地址提供及维护费用（市场价3600元/年;',
					'2、刻章费用（公章、法人章、财务章、发票章）（市场价450元）',
					'3、税盘费用（市场价481元）',
					'4、银行基本户开设及网银盾费用（市场价700元）',
					'5、电子发票软件费用（市场价380元）',
					'6、工商年报代办费用（市场价200元/年）',
					'7、税务汇算清缴费用（市场价200元/年）',
					'7、税务汇算清缴费用（市场价200元/年）',
					'8、代理记账及领取发票费用（市场价3600元/年）',
					'*   该费用包含首年各项服务及材料成本费用共1999元',
					'*   724平台首批100名用户优惠价只需999元',
					'*   次年起费用为999/年',
				],
				packageActiveIndex : 1,
				packageActiveTxt : '一',
				money : '',
				package1 : '',
				package2 : ''
			};
		},
		onLoad() {
			// console.log('查看innercode',uni.getStorageSync('innercode'));
			_self = this;
			uni.request({
				url: global.host + 'Zhu/clearCharacterTimeout',
				method: 'GET',
				data: {},
				success: res => {
					console.log('清理过期的字号',res);
				},
				fail: () => {},
				complete: () => {}
			});
			
			const character = uni.getStorageSync('zihao');
			uni.request({
				url: global.host + 'Zhu/getInfo',
				method: 'GET',
				data: {
					character : character
				},
				success: res => {
					if(res.data == '没有该字号') {
						// 说明字号已经过期
						console.log('waitpayonlaod没有该字号');
					} else {
						account = res.data[0].account;
						password = res.data[0].password;
					}
						
				},
				fail: () => {},
				complete: () => {}
			});
			
			uni.request({
				url: global.host  + 'Zhu/getController',
				method: 'GET',
				data: {},
				success: res => {
					// package1 是724的价格
					console.log('套餐',res.data[0].package1); // 724的
					console.log('套餐',res.data[0].package2); // 普通的
					_self.package1 = res.data[0].package1/100;
					_self.package2 = res.data[0].package2/100;
					// 默认价格为普通的package2
					_self.money = res.data[0].package2/100; 
					const main_scope = uni.getStorageSync('main_scope');
					console.log(main_scope)
					if(main_scope == '健康销售') {
						// 说明是724用户,套餐价格是999
						console.log('进来了套餐',res.data[0].package1/100);
						_self.money = res.data[0].package1/100; 
					}
					// 获取用户信息
					try {
						const userinfo = uni.getStorageSync('userinfo');
						
						if (userinfo) {
							user_name = userinfo.user_name;
							user_phone = userinfo.user_phone;
							console.log('userinfo',user_name,user_phone);
						}
					} catch (e) {
						// error
					}
				},
				fail: () => {},
				complete: () => {}
			});
			
			
			
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
			reallypost() {
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
									let bussiness_name = uni.getStorageSync('bussiness_name');
									let business_scope = uni.getStorageSync('business_scope'); // 经营范围数组类型
									let business_scope_text = uni.getStorageSync('business_scope_text'); // 经营范围文本类型

									
									// 要判断登录那个账号 并判断是在第一页 还是在更多
									// 传字号
									// 先查看这个character是否存在,在那个账号下,返回这个账号的信息
									try {
										const character = uni.getStorageSync('zihao');
										uni.request({
											url: global.host + 'Zhu/getInfo',
											method: 'GET',
											data: {
												character : character
											},
											success: res => {
												console.log('查看数据',res);
												let dataLength = res.data.length;
												if(res.data == '没有该字号') {
													// 说明字号已经过期
													
													uni.showModal({
														title: '提示',
														content: '为你保留24小时字号已过期!请清理缓存重试!',
														success: function (res) {
															if (res.confirm) {
																console.log(777);
																
															} else if (res.cancel) {
																console.log('用户点击取消');
															}
														}
													});
												} else {
													
													let {account, password} = res.data[0];
													if(dataLength <= 5) {
														// 说明没有更多按钮 请求新的函数计算(没有更多的)
														aisle = '1';
													} else {
														// 说明有更多的按钮 
														aisle = '2';
													}
													
														// let infofront = uni.getStorageSync('infofront');
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
																	uni.request({
																		url: global.host + 'Zhu/getUserInfo',
																		method: 'GET',
																		data: {
																			openid : global.openid
																		},
																		success: res => {
																			let {user_name,user_phone} = res.data[0];
																			// 提交核名
																			uni.showLoading({
																				title: '办理核名中...'
																			});
																			let random = (Math.floor(Math.random()*9999)+0) + '';
																			if(random.length < 5) {
																				let num = 5 - random.length;
																				for(let i = 0; i < num; i++) {
																					random = '0' + random
																				}
																			}
																			let S_OPSCOPE = encodeURIComponent(global.business_scope_return(business_scope_text)).replace(/25+/g,'');
																			let S_DOM_LOCATION = encodeURIComponent('惠明路82号五楼501-'+random+'号').replace(/25+/g,'');
																			let S_FDDBR = encodeURIComponent(user_name).replace(/25+/g,'');
																			let globalName = encodeURIComponent(bussiness_name.replace('名称查重通过', '')).replace(/25+/g,'');
																			console.log(999)
																			console.log(global.host + 'Zhu/sendName?S_OPSCOPE=' + S_OPSCOPE + '&S_DOM_LOCATION=' + S_DOM_LOCATION + '&S_FDDBR=' + S_FDDBR + '&globalName=' + globalName);
																			console.log(888)
																			let url = global.host + 'Zhu/sendName?S_OPSCOPE=' + S_OPSCOPE + '&S_DOM_LOCATION=' + S_DOM_LOCATION + '&S_FDDBR=' + S_FDDBR + '&globalName=' + globalName;
																			// let url = 'http://crm.binbin.aiyongbao.com/Zhu/sendName?S_OPSCOPE=' + S_OPSCOPE + '&S_DOM_LOCATION=' + S_DOM_LOCATION + '&S_FDDBR=' + S_FDDBR + '&globalName=' + globalName;
																			if(aisle == '1') {
																				console.log('走的是没有更多按钮的函数计算');
																				// url = 'http://crm.binbin.aiyongbao.com/Zhu/sendNameNoMore?S_OPSCOPE=' + S_OPSCOPE + '&S_DOM_LOCATION=' + S_DOM_LOCATION + '&S_FDDBR=' + S_FDDBR + '&globalName=' + globalName;
																				url = global.host + 'Zhu/sendNameNoMore?S_OPSCOPE=' + S_OPSCOPE + '&S_DOM_LOCATION=' + S_DOM_LOCATION + '&S_FDDBR=' + S_FDDBR + '&globalName=' + globalName;
																			}
																			uni.request({
																				url: url,
																				method: 'GET',
																				data: {
																					S_MOBTEL : user_phone,
																					account : account,
																					password : password
																				},
																				success: res => {
																					uni.hideLoading();
																					console.log('提交查询',res);
																					if(res.data.indexOf('您申请的') == -1) {
																						// 不存在,说明失败
																						uni.showModal({
																							title: '提示',
																							content: '核名出错:'+res.data,
																							success: function (res) {
																								if (res.confirm) {
																									console.log('用户点击确定');
																								} else if (res.cancel) {
																									console.log('用户点击取消');
																								}
																							}
																						});
																						return;
																					}
																					uni.setStorageSync('beian', res);
																					uni.request({
																						// url: 'http://crm.binbin.aiyongbao.com/Zhu/setNotification',
																						url: global.host + 'Zhu/setNotification',
																						method: 'GET',
																						data: {
																							notification : res,
																							openid : global.openid
																						},
																						success: res => {
																							console.log('设置告知书到库',res);
																						},
																						fail: () => {},
																						complete: () => {}
																					});
																					// 核名成功以后需要将字号库对应的字号删除
																					try {
																						const zihao = uni.getStorageSync('zihao');
																						if (zihao) {
																							uni.request({
																								url: global.host + 'Zhu/deleteZihao',
																								method: 'GET',
																								data: {
																									character : zihao
																								},
																								success: res => {
																									console.log('删除成功',res);
																									try {
																											console.log('支付成功修改了step时候跳转',res);
																										uni.redirectTo({
																											url: '../main_index/main_index?from=waitPay'
																										});
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
																					}
																				},
																				fail: (e) => {
																					console.log(e);
																					console.log('提交核名错误',e);
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
																	
																	
																},
																fail: () => {},
																complete: () => {}
															});
														}
														
													}
												}
												
												
												
											},
											fail: () => {},
											complete: () => {}
										});
									} catch (e) {
										// error
										console.log('获取缓存字号失败');
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
			},
			payMoney() {
				_self = this;
				// 目前认为是支付成功
				// 支付成功要修改数据库 公司的
				// 开始支付
				console.log('点击了支付');
				// 在支付前,需要判断字号是否已经过期
				
				uni.request({
					url: global.host + 'Zhu/getController',
					method: 'GET',
					data: {},
					success: res => {
						// 在支付的时候 确认是否维护
						if(res.data[0].switch10 != 1) {
							// 在维护
							uni.showLoading({
								title: '第一批邀请测试结束，敬请期待下一次开放。'
							});
							setTimeout(()=>{
								  uni.hideLoading();
							},2000)
							
						} else {
							const character = uni.getStorageSync('zihao');
							uni.request({
								url: global.host + 'Zhu/getInfo',
								method: 'GET',
								data: {
									character : character
								},
								success: res => {
									if(res.data == '没有该字号') {
										// 说明字号已经过期
										uni.showModal({
											title: '提示',
											content: '为你保留24小时字号已过期!请清理缓存重试!',
											success: function (res) {
												if (res.confirm) {
													uni.navigateBack({
														delta: 1
													});
												} else if (res.cancel) {
													console.log('用户点击取消');
												}
											}
										});
									} else {
										try {
											const is_pay = uni.getStorageSync('is_pay');
											if (is_pay) {
												if(is_pay == 'yes') {
													// 已经支付过,直接提交
													// 调用提交核名函数
													
													
													_self.reallypost();
													return;
												}
											}
										} catch (e) {
											// error
										}
										
										let time = new Date().getTime();
										let reallymoney = _self.money+'00';
										if(uni.getStorageSync('innercode')) {
											if(uni.getStorageSync('innercode') == '334455') {
												console.log('查看innercode',uni.getStorageSync('innercode'));
												typeuser = '724';
												reallymoney = '1';
											}
										}
										uni.request({
											url: global.host + 'Zhu/md5binbin',
											method: 'GET',
											data: {
												out_trade_no : time + '',
												total_fee : reallymoney,
												appid : 'wx08c77d4e3bb67d91',
												mch_id : '1529227481',
												openid : global.openid, 
											},
											success: res => {
												console.log('第一次签名',res);
												//支付成功以后将信息保存起来
												let ordernum = time;
												let total_fee = total_fee;
												
												let prepay_id = res.data.prepay_id;
												uni.request({
													url: global.host + 'Zhu/secondmd5',
													method: 'GET',
													data: {
														appId : 'wx08c77d4e3bb67d91', 
														nonceStr : '5K8264ILTKCH16CQ2502SI8ZNMTM67VS',
														package : 'prepay_id=' + prepay_id,
														timeStamp : time + '',
													},
													success: res => {
														console.log('再次签名',res);
														console.log('prepay_id',prepay_id);
															uni.requestPayment({
															provider: 'wxpay',
															timeStamp: time + '',
															nonceStr: '5K8264ILTKCH16CQ2502SI8ZNMTM67VS',
															package: 'prepay_id=' + prepay_id,
															signType: 'MD5',
															paySign: res.data,
															success: function (res) {
																console.log('支付的返回结果success',res);
																if(res.errMsg == 'requestPayment:ok') {
																	uni.showToast({
																		title: '支付成功',
																		duration: 2000
																	});
																	 const zihao = uni.getStorageSync('bussiness_name');
																	 console.log(reallymoney,decodeURIComponent(user_name),typeuser,time)
																	uni.request({
																		// url: 'http://crm.binbin.aiyongbao.com/Zhu/setOrder',
																		url: global.host + 'Zhu/setOrder',
																		method: 'GET',
																		data: {
																			pay_money : reallymoney,
																			name : decodeURIComponent(user_name),
																			type : typeuser, // 1为普通用户
																			user_where : '普通',
																			package : '1',
																			order_no : time,
																			openid : global.openid,
																			phone : user_phone,
																			zihao : zihao,
																			account : account,
																			password : password
																			
																		},
																		success: res => {
																			console.log('查看设置支付订单',res);
																			// 设置缓存说明支付成功
																			try {
																				uni.setStorageSync('is_pay', 'yes');
																			} catch (e) {
																				// error
																			}
																			
																			// 调用提交核名函数
																			_self.reallypost();
																		},
																		fail: () => {},
																		complete: () => {}
																	});
																	
																}
																console.log('success:' + JSON.stringify(res));
															},
															fail: function (err) {
																uni.showToast({
																	title: '支付失败',
																	duration: 2000,
																	icon : 'none'
																});
																console.log('支付的返回结果err',res);
																console.log('fail:' + JSON.stringify(err));
															}
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
								},
								fail: () => {},
								complete: () => {}
							});
						}
					},
					fail: () => {},
					complete: () => {}
				});
				
				
			
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
	margin-left: 50upx;
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
