<!-- 注册登记  基本信息 -->
<template>
	<view class="registration">
		<view class="registration_name">
			<text>基本信息</text>
		</view>
		<!-- 注册资金 手机号 -->
		<view :class="isDisplayVerificationCode == true ? 'registration_block_add' : 'registration_block'">
			<!-- <view class="registration_block_1">
				<text>注册资金</text>  
				<picker @change="bindPickerChange" :value="index" :range="array">
					<view class="" style=" padding-left: 10upx; padding-right: 10upx;"><text style="display: inline-block; width: 50upx; border-bottom: 1px solid #E5E5E5; margin-left: 5upx; margin-right: 5upx;">{{array[index]}}</text>万元</view>
				</picker>
			</view> -->
			<view class="registration_block_2">
				手机号 <input type="text"  @blur="vertifyPhone"  :disabled="isDisplayVerificationCode == true ? false :true "  :value="phone" /> <text @click="changePhone" :class="isDisplayVerificationCode == true ? 'verificationCodeStyle' : ''" style="color: #4A90E2;">{{codeTime}}{{phoneTxt}}</text> 
			</view>
			<view :class="isDisplayVerificationCode == true ? 'registration_block_3':'no_registration_block_3'">
				验证码 <input type="text"  @blur="vertifyCode" :value="verificationCode" /> <text @click="" style="color: #FF0000;">{{vertifyTxt}}</text>
			</view>
		</view>
		
		<!-- 实名认证 -->
		<view class="registration_title">
			<text>实名认证</text>
		</view>
		
		<!-- 身份证正反面 -->
		<view class="registration_card">
			<view class="id_block">
				<image class="id_block_add" @click="clickFront(1)" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/add.png" mode=""></image>
				<text>身份证人像面</text>
				<image class="card_img" :src="imgfrontsrc" mode=""></image>
			</view>
			<view class="id_block">
				<image class="id_block_add" @click="clickFront(2)" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/add.png" mode=""></image>
				<text>身份证国徽面</text>
				<image  class="card_img" :src="imgbacksrc" mode=""></image>
			</view>
			
		</view>
		<!-- 信息确定 -->
		<view class="registration_ok">
			<!-- <view class="registration_ok_left">
				<image class="registration_ok_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/pre_step.png" mode=""></image>
				<text class="registration_ok_txt">重新名称查重</text>
			</view> -->
			<view class="registration_ok_right" @click="goRegistrationInfo">
				<image class="registration_ok_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/right_icon.png" mode=""></image>
				<text class="registration_ok_txt">下一步</text>
			</view>
			
		</view>
		
		
	</view>
</template>

<script>
	var _self;
	export default {
		data() {
			return {
				phone : '',
				verificationCode : '',
				array: ['1', '10', '100'],
				index: 0,
				isDisplayVerificationCode : false, //是否显示验证码这一栏,默认不显示
				phoneTxt : '修改',
				codeTime: '', //默认信息反应的时间为60s
				waitCode : '', //给用户发送的短信验证码
				vertifyTxt : '',
				imgfrontsrc : 'http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/id_front_img.png',
				imgbacksrc : 'http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/id_back_img.png',
				viewDisable : false, // 页面内是否可以操作, 比如在上传图片的时候就不能操作了
				nosendcode : true, // 不能发送验证码
			};
		},
		onLoad() {
			// 获取手机号,首先查看该用户下面有没有手机号
			if(global.openid) {
				// 存在openid
				uni.request({
					url :  global.host + 'Zhu/getUserInfo?openid=' + global.openid,
					method: 'GET',
					data: {},
					success: res => {
						console.log('registration onload的时候获取用户信息',res);
						if(res.data[0].user_phone) {
							this.phone = res.data[0].user_phone;
						} else {
							
						}
					},
					fail: () => {},
					complete: () => {}
				});
			}
		},
		methods: {
			clickFront(type) {
				console.log('viewDisable',this.viewDisable);
				if(this.viewDisable == true) return false;
				if(type == 1) {
					// 正面
					console.log('点击了正面',type);
				} else if(type == 2) {
					// 反面
					console.log('点击了反面',type);
				}
				_self = this;
				uni.chooseImage({
					count: 1, //默认9
					 sizeType: ['compressed'],//可以指定是原图还是压缩图，默认二者都有
					success: function (res) {
						console.log(JSON.stringify(res));
							// 上传中禁止其他操作  
							_self.viewDisable = true;
							uni.showLoading({
								title: '上传中...'
							});
							console.log('上传的res',res);
						     const tempFilePaths = res.tempFilePaths;
							 console.log('图片路径',tempFilePaths[0]);
							 if(type == 1){
								 _self.imgfrontsrc = tempFilePaths[0];
							 } else if(type == 2) {
								 _self.imgbacksrc = tempFilePaths[0];
							 }
							 const uploadTask  = uni.uploadFile({
							  url :  global.host + '/Zhu/upload',
							  filePath : tempFilePaths[0],
							  name: 'file',
							  formData : {
								'user' : 'test'
							  },
							  success: function (uploadFileRes) {
							   console.log('上传反馈',uploadFileRes);
							   console.log('上传反馈的数据',JSON.parse(uploadFileRes.data));
							   _self.viewDisable = false;
							    uni.hideLoading();
							   let uploadFileRes1 = JSON.parse(uploadFileRes.data);
							   let imgurl = uploadFileRes1.msg;
							   // 将正面的base64存到本地
							   try {
								   if(type == 1) {
									   uni.setStorageSync('base64front', uploadFileRes1.base64);
									   uni.setStorageSync('imgfront',imgurl);
								   } else if(type == 2) {
									   uni.setStorageSync('base64back', uploadFileRes1.base64);
									   uni.setStorageSync('imgback',imgurl);
								   }
								} catch (e) {
									// error
									console.log('error-base64front');
								}
							   
							  }
							 });
							 uploadTask.onProgressUpdate(function (res) {
							  _self.percent = res.progress;
							  console.log('上传进度' + res.progress);
							  console.log('已经上传的数据长度' + res.totalBytesSent);
							  console.log('预期需要上传的数据总长度' + res.totalBytesExpectedToSend);
							 });
					}
				});
			},
			bindPickerChange: function(e) {
				console.log('picker发送选择改变，携带值为', e.target.value)
				this.index = e.target.value
			},
			changePhone(e) {
				if(this.nosendcode == false) {
					uni.showToast({
						title: '手机号不正确',
						duration: 1000,
						icon : 'none'
					});
					return false;
				}
				// 判断点击的是 修改 还是 获取验证码
				if(this.viewDisable == true) return false;
				if(this.phoneTxt == '修改') {
					// 修改手机号,要显示验证码这一栏
					this.isDisplayVerificationCode = true;
					this.phoneTxt = '获取验证码';
					console.log('修改号码');
				} else if(this.phoneTxt == '获取验证码' || this.phoneTxt == '重新获取验证码') {
					if(this.phone.length != 11) {
						return false;
					}
					// 需要给用户的手机号发送短信
					let code = global.getVeritifyCode(); // 这个函数在全局
					let mobile = this.phone; //将要绑定的手机号,即发送的手机号
// 					let tpl_id = 140513; //模板号 模板就是发送信息的模板,模板是聚合数据的模板
// 					let tpl_value = '%23code%23%3D' + code;
// 					let key = '8972e94284e4fb4fafc3266c8834d25c'; //聚合数据的appkey
// 					let url = "http://v.juhe.cn/sms/send?mobile=" + mobile + '&tpl_id=' + tpl_id + '&dtype=&key=' +  key + '&tpl_value=' + tpl_value; 
					uni.request({
						url: global.host + 'Zhu/getPhoneCode', 
						data:{
								code : code,
								mobile : mobile
						},
						success: (res) => {
							if(res.data.reason == "操作成功") {
								this.waitCode = code; 
								// 信息发送成功
								this.codeTime = 60;
								this.phoneTxt = '后重新获取';
								var timer = setInterval(()=> {
									this.codeTime--;
									if(this.codeTime  <= 0) {
										console.log('99',this.codeTime);
										clearInterval(timer);
										this.codeTime = '';
										this.phoneTxt = '重新获取验证码';
									}
								},1000);
							}
						}
					});
				}
				
			},
			// 验证手机验证码
			vertifyCode(e) {
				_self = this;
				// 判断用户输入的验证码与发送的验证码是否一致
				if(e.detail.value == this.waitCode && e.detail.value !='') {
					this.vertifyTxt = '验证成功';
					this.codeTime = 0;
					// this.phoneTxt = '重新取验证码';
					
					try {
						const openid = uni.getStorageSync('openid');
						if (openid) {
							console.log('user_phone',_self.phone);
							uni.request({
								url: global.host + 'Zhu/changePhone',
								method: 'GET',
								data: {
									openid : openid,
									user_phone : _self.phone
								},
								success: res => {
									uni.showToast({
										title: '修改成功了',
										duration: 2000
									});
									console.log('修改手机号成功',res);
									setTimeout(() => {
										this.isDisplayVerificationCode  = false;
										this.phoneTxt = '修改';
										this.vertifyTxt = '';
										this.verificationCode = '';
									},2000)
								},
								fail: () => {},
								complete: () => {}
							});
						}
					} catch (e) {
						// error
					}
				} else if(this.phoneTxt == '获取验证码'){
					uni.showToast({
						title: '请先获取验证码',
						duration: 2000,
						image : '../../static/icon/error.png'
					});
				} else {
					this.vertifyTxt = '验证码错误';
				}
			},
			goRegistrationInfo() {
				if(this.viewDisable == true) return false; // 如果有任务进行,不能进行其他操作.
				// 先判断缓存是否有 frontinfo 与 backinfo 
				try {
					const base64front = uni.getStorageSync('base64front'); // 身份证正面base64
					const base64back = uni.getStorageSync('base64back'); // 身份证反面base64
					const imgfront = uni.getStorageSync('imgfront'); // 身份证正面的地址
					const imgback = uni.getStorageSync('imgback'); // 身份证反面的地址
					if(this.phone == '') {
						uni.showToast({
							title: '你点击修改,绑定手机号',
							image : '../../static/icon/error.png',
							duration: 2000
						});
						return false;
					}
					if(base64front && base64back && imgfront && imgback) {
						// 信息都存在的情况
						uni.showLoading({
							title: '正面信息读取中'
						});
					   uni.request({
						url: global.host + 'Zhu/getInfoID',
						method: 'POST',
						header: {
							'content-type': 'application/x-www-form-urlencoded', 
						},
						data: {
							image : base64front,
							side : 'front'
						},
						success: res => {
							uni.hideLoading();
							console.log('正面身份证信息',res);
								const {idcard,realname,address,born} = res.data.result;
								if(idcard == '' || realname == '' || address == '' || born == ''){
									uni.showModal({
										title: '认证失败',
										content: '正面身份证模糊,请重新上传',
										showCancel : false,
										success: function (res) {
											if (res.confirm) {
												console.log('用户点击确定');
											} else if (res.cancel) {
												console.log('用户点击取消');
											}
										}
									});
									return false;
								}
								// 将正面的信息存到缓存
								try {
									uni.setStorageSync('infofront', res);
								} catch (e) {
									// error
									console.log('设置缓存失败infofront');
								}
								if(res) {
									uni.request({
										url: global.host + 'Zhu/verificationRealName',
										method: 'GET',
										data: {
											idcard : idcard,
											realname : realname
										},
										
										success: res => {
											console.log('查看实名情况',res);
											if(res.data.msg == '身份证号码和真实姓名不一致') {
												console.log('实名未通过');
												uni.showModal({
													title: '提示',
													content: '身份证正面信息模糊,请重新上传正面身份证',
													success: function (res) {
														if (res.confirm) {
															console.log('用户点击确定');
														} else if (res.cancel) {
															console.log('用户点击取消');
														}
													}
												});
												// 实名未通过
											} else if(res.data.msg == '身份证号码和真实姓名一致') {
												// 实名通过
												console.log('实名通过');
													uni.showLoading({
													title: '反面信息读取中'
												});
												uni.request({
													url: global.host + 'Zhu/getInfoID',
													method: 'POST',
													header: {
														'content-type': 'application/x-www-form-urlencoded', 
													},
													data: {
														image : base64back,
														side : 'back'
													},
													success: res => {
														uni.hideLoading();
														console.log('反面身份证信息',res);
														let {begin,department,end,side} = res.data.result;
														if(begin.length != 8 || end.length!= 8 || side != "back" || department == '') {
															uni.showModal({
																title: '提示',
																content: '背面身份证验证失败,请重新上传',
																success: function (res) {
																	if (res.confirm) {
																		console.log('用户点击确定');
																	} else if (res.cancel) {
																		console.log('用户点击取消');
																	}
																}
															});
														} else {
															try {
																uni.setStorageSync('infoback', res);
															} catch (e) {
																console.error('infobackerror');
															}
															let infoback = res;
															// 获取正面信息
															try {
																const infofront = uni.getStorageSync('infofront');
																const imgfront = uni.getStorageSync('imgfront');
																const imgback = uni.getStorageSync('imgback');
																if (infofront) {
																	console.log('正面与反面都认证成功');
																	if(global.openid) {
																		let {realname, sex, nation, born, address, idcard} = infofront.data.result;
																		if(sex == '男') sex = 1;
																		if(sex == '女') sex = 2;
																		uni.request({
																			url: global.host + 'Zhu/changeIdCard',
																			method: 'GET',
																			data: {
																				openid : global.openid,
																				user_name : realname,
																				id_card_number : idcard,
																				id_card_sex : sex,
																				id_card_nation : nation,
																				id_card_address : address,
																				id_card_front : imgfront,
																				id_card_back : imgback
																			},
																			success: res => {
																				// 正面与反面都认证成功
																				// 需要最后验证一下 正反身份证是否匹配
																				// 背面身份证去除 公安局 然后看地址是否在正面的身份证地址存在
																				// if(infofront)
																				console.log('back',infoback.data.result.department.split('公安局')[0]);
																				let match = infoback.data.result.department.split('公安局')[0];
																				console.log(infofront.data.result.address.indexOf(match) != -1);
																				if(infofront.data.result.address.indexOf(match) != -1) {
																					uni.navigateTo({
																						url: '../registrationInfo/registrationInfo'
																					});																			
																				} else {
																					uni.showModal({
																						title: '提示',
																						content: '身份证正反面不匹配,请重新上传',
																						success: function (res) {
																							if (res.confirm) {
																								console.log('用户点击确定');
																							} else if (res.cancel) {
																								console.log('用户点击取消');
																							}
																						}
																					});
																				}

																			},
																			fail: () => {},
																			complete: () => {}
																		});
																	} else {
																		console.log('registration_error');
																	}
																}
															} catch (e) {
																// error
															}
														
															
															
															
															
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
							
						},
						fail: () => {},
						complete: () => {}
					   });
					} else {
						uni.showToast({
							title: '请先上传身份证',
							duration: 2000,
							image : '../../static/icon/error.png'
						});
					}
				} catch (e) {
					// error
				}
			},
			vertifyPhone(e) {
				let phone = e.detail.value;
				if(phone.length != 11) {
					uni.showToast({
						title: '手机号不正确',
						duration: 1000,
						image : '../../static/icon/error.png'
					});
					this.nosendcode = false;
				} else {
					this.nosendcode = true;
					console.log('89',e.detail.value);
					this.phone = e.detail.value;
				}
			}
		}
	}
</script>

<style>
.verificationCodeStyle {
	/* color: ; */
}
.id_block_add {
	position: absolute;
	top: 110px;
	left: 250upx;
	width: 100upx;
	height: 50px;
}
.id_block {
	position: relative;
}
.card_img {
	margin-top: 15px;
	width: 600upx;
	height: 190px;
}
.registration {
	text-align: center;
}
.registration_name {
	font-size: 15px;
	margin-top: 10px;
	margin-bottom: 10px;
}
.registration_block {
	margin: 0 auto;
	width: 674upx;
	height: 42px;
	border: 1px solid #E5E5E5;
	border-radius: 5px;
}
.registration_block_add {
	margin: 0 auto;
	width: 674upx;
	height: 84px;
	border: 1px solid #E5E5E5;
	border-radius: 5px;
	/* transition: height 500ms; */
}
.registration_block_1 {
	padding-left: 25upx;
	border-bottom: 1px solid #E5E5E5;
	height: 42px;
	line-height: 42px;
	display: flex;
	flex-direction: row;
	align-items: center;
}
.registration_block_2 {
	padding-left: 50upx;
	/* margin-left: 50upx; */
	height: 42px;
	line-height: 42px;
	display: flex;
	flex-direction: row;
	align-items: center;
	/* background: #E5E5E5; */
}
.registration_block_3 {
	text-align: center;
	border-top: 1px solid #E5E5E5;
	padding-left: 50upx;
	height: 42px;
	line-height: 42px;
	display: flex;
	flex-direction: row;
	align-items: center;
}
.no_registration_block_3 {
	display: none;
}
.registration_title {
	margin: 0 auto;
	margin-top: 20px;
	font-size: 15px;
}

.registration_card {
	width: 602upx;
	margin: 0 auto;
	text-align: left;
	color: #B2B2B2;
	font-size: 13px;
}
.registration_ok {
	display: flex;
	flex-direction: row;
	justify-content: space-around;
	margin: 0 auto;
	margin-top: 55px;
	height: 80px;
}
.registration_ok_left {
	display: flex;
	flex-direction: column;
}
.registration_ok_right {
	display: flex;
	flex-direction: column;
}
.registration_ok_img {
	margin: 0 auto;
	width: 40upx;
	height: 20px;
}
.registration_ok_txt {
	margin-top: 10px;
	font-size: 14px;
}

</style>
