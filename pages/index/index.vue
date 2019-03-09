<template>
	<view class="content">
		<!-- 步骤介绍 -->
		<view class="step">
			<text class="stepTitle">
				简单5步在线申请个体营业执照
			</text>
		</view>
		
		<!-- 宣传区域 -->
		<view class="promotionArea">
			
		</view>
		
		<!-- 立即申请 -->
		<!-- <view class="applyImmediately">
			<button type="" size="mini" plain="true" style="width: 360upx; color: #1AAD19; border-color: #1AAD19;" hover-class = "btn_hover">
				立即申请
			</button>
		</view> -->
		
		<!-- #ifdef MP-WEIXIN  -->
		<!-- <view class="applyImmediately">
			<button  size="mini" plain="true" style="width: 360upx; color: #1AAD19; border-color: #1AAD19;"  hover-class = "btn_hover" open-type="getPhoneNumber" @getphonenumber="getPhoneNumber">立即申请</button>
		</view> -->
		<!-- #endif -->  
		
		<!-- #ifdef MP-WEIXIN  -->
		<view class="applyImmediately">
			<button  size="mini" plain="true" style="width: 360upx; color: #1AAD19; border-color: #1AAD19;"  hover-class = "btn_hover" open-type="getUserInfo" @getuserinfo="getuserinfo">微信登录</button>
		</view>
		<!-- #endif -->  
		<!-- 常见问题 -->
		<view class="">
			<text class="commonProblem">
				常见问题
			</text>
		</view>
		
	</view>
</template>

<script>
	var _left;
	export default {
		data() {
			return {
				activeIndex : 0,
				news : [
				],
				music : "http://img-cdn-qiniu.dcloud.net.cn/uniapp/audio/music.mp3",
				zhangbin : [
					{ name : 'zhangbin', sex : '男'},
					{ name : '小晨', sex : 'girl'},
					{ name : '小丁', sex : 'girl'}
				],
				age : 20,
				text : true,
				isRed : true,
				menus : [
					'新闻',
					'图片',
					'音乐'
				]
			}
		},
	
		onLoad() {
			
// 			uni.request({
// 				url: global.host + 'Zhu/demo',
// 				method: 'GET',
// 				data: {},
// 				success: res => {
// 					console.log('99',res);
// 				},
// 				fail: () => {},
// 				complete: () => {}
// 			});
			_left = this;
			
			var res = global.isLogin();
			// 如果存在openid 说明已经登录过
			  if(res){
				  try {
						const openid = uni.getStorageSync('openid');
						if (openid) {
							 uni.request({
							 	url: global.host + 'Zhu/getCurrentStep',
							 	method: 'GET',
							 	data: {
									openid : openid 
								},
							 	success: res => {
									console.log('在idnex下查看进度',res);
									let current_step = res.data.res[0].current_step;
									if(current_step == '88') {
										// 到选择身份
										console.log('目前处于选择身份阶段');
										uni.redirectTo({
											url: '../choiceIdentity/choiceIdentity'
										});	
									} else {
										console.log('目前已过选择身份阶段');
										uni.redirectTo({
											url: '../main_index/main_index'
										});	
									}
								},
							 	fail: () => {},
							 	complete: () => {}
							 });
						}
					} catch (e) {
						// error
					}
			  }

		},
		onShow() {
			console.log('页面显示1');
		},
		onHide() {
			console.log('页面隐藏1');
		},
		methods: {
			openinfo(e) {
				var newsid = e.currentTarget.dataset.newsid;
				uni.navigateTo({
					url: '../info/info?newsid=' + newsid
				});
			},
			menuClick (e) {
				console.log(e.currentTarget.id);
				this.activeIndex = e.currentTarget.id;
			},
			img () {
				uni.chooseImage({
					count : 3,
					sizeType : "compressed",
					success : function(res) {
						console.log(res);
					}
				})
			},
			getPhoneNumber(e) {
				console.log('手机号')
				console.log(e)
				console.log(e.detail.errMsg)
				console.log(e.detail.iv)
				console.log(e.detail.encryptedData)
			},
			getuserinfo(e) {
				console.log('查看点击了同意以后输出了什么',e);
				if(e.detail.errMsg == "getUserInfo:ok") {
					uni.showToast({
						title: '你已授权,进入功能界面',
						duration: 2000,
						icon : 'none'
					});
					// 说明用户点击了同意
					var userInfo = e.detail.userInfo; // userInfo下面有 avatarUrl city country gender  language nickName province
					try {
						uni.setStorageSync('nickName', userInfo.nickName);
						uni.setStorageSync('avatarUrl', userInfo.avatarUrl);
					} catch (e) {
						// error
					}
					console.log(333);
					uni.login({
					  provider: 'weixin',
					  success: function (res2) {
						console.log('res2',res2);
						
						uni.request({
							url: global.host + 'Zhu/getOpenid',
							method: 'GET',
							data: {
								code : res2.code
							},
							success: res => {
								console.log(res);
							},
							fail: () => {},
							complete: () => {}
						});
						
						return false;
						uni.request({
							url: global.host + 'Zhu/getOpenid',
							method: 'GET',
							data: {
								code : res2.code
							},
							success: res => {
								// 得到openid
								console.log('res999',res);
								try {
									uni.setStorageSync('openid', res.data);
									let openid = res.data.openid;
									// 存储完以后跳转到选择身份 选择身份阶段为 88
									// 发送请求 存储用户信息,且修改当前阶段在 选择身份(88)
									uni.request({
										url: global.host + 'Zhu/getUserInfo?openid=' + openid,
										method: 'GET',
										data: {},
										success: res => {
											console.log('查询用户信息是否存在',res);
											if(res.data.length >= 1) {
												// 说明用户已经在用户表内,就不需要再注册
												console.log('用户数据已经在数据表了');
											} else {
												// 用户信息没有在数据表 插入新数据
												uni.request({
													url: global.host + 'Zhu/insertUserInfo?openid=' + openid,
													method: 'GET',
													data: {},
													success: res => {
														console.log('新建用户数据到数据库',res);
													},
													fail: () => {},
													complete: () => {}
												});
											}
											// 首选查看用户是否有step 有了就不需要再操作
											uni.request({
												url: global.host + 'Zhu/getCurrentStep',
												method: 'GET',
												data: {
													openid : openid 
												},
												success: res => {
													let current_step = res.data.res[0].current_step;
													console.log('查看index时候的current_step',current_step);
													if(current_step!= '888') {
														// 说明已经存在current_step 不用修改88
														uni.redirectTo({
															url: '../main_index/main_index'
														});
													} else {
														uni.redirectTo({
															url: '../choiceIdentity/choiceIdentity'
														});
														
														// 修改该用户的setp
// 														uni.request({
// 															url: global.host + 'Zhu/editCurrentStep',
// 															method: 'GET',
// 															data: {
// 																openid : openid,
// 																current_step : 88 // 88 对应的是选择身份
// 															},
// 															success: res => {
// 																console.log('查看修改进度的返回结果',res);
// 																uni.redirectTo({
// 																	url: '../choiceIdentity/choiceIdentity'
// 																});
// 															},
// 															fail: () => {},
// 															complete: () => {}
														// });
													}
												},
												fail: () => {},
												complete: () => {}
											});
											
										
										},
										fail: () => {},
										complete: () => {}
									});
									
// 									uni.redirectTo({
// 										url: '../choiceIdentity/choiceIdentity'
// 									});
								} catch (e) {
									// error
								}
							},
							fail: (e) => {
								console.log('error',e);
							},
							complete: (e) => {
								console.log('343',e);
							}
						});
					  }
					});
					
				} else {
					uni.showToast({
						title: '你未授权,不能进行下一步',
						duration: 2000,
						icon : 'none'
					});
				}


			},
		},
	}
</script>

<style>
	.uni-media-list-body{height: auto;}
	.uni-media-list-text-top{line-height: 1.6em;}
	.menus {
		float: left;
		margin-left: 10upx;
	}
	.activeStyle {
		color: red;
	}
	
	.content {
		font-style: normal;
		text-align: center;
	}
	
	.step {
		margin-top: 25px;
		margin-bottom: 25px;
		text-align: center;
	}
	.stepTitle {
		font-size: 17px;
	}
	.promotionArea {
		margin:  0 auto;
		width: 670upx;
		height: 429px;
		background: #D8D8D8;
	}
	
	/* 立即申请 */
	.applyImmediately {
		margin-top: 25px;
		margin-bottom: 25px;
	}
	.btn_hover {
		color: black!important;
	}
	
	/* 常见问题 */
	.commonProblem {
		color: #BD10E0;
		font-size: 10px;
	}
</style>
