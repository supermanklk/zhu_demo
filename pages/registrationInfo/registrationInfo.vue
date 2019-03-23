<!-- 注册登记 注册全部信息 -->
<template>
	<view class="registrationInfo">
		
		<!-- 预览营业执照 -->
		<view :class="alertImg == true ? 'registrationInfo_alert_img' : 'no_registrationInfo_alert_img' ">
			<!-- <image src="" mode=""></image> -->
		</view>
		<view :class="alertImg == true ? 'registrationInfo_alert_img_x' : 'no_registrationInfo_alert_img_x'">
			<image class="registrationInfo_alert_img_x_item" @click="closeImgAlert" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/error.png" mode=""></image>
		</view>
		<view :class="alertImg == true ? 'mengceng' : ''">
		</view>
		
		<!-- 查看弹窗-协议 -->
		<view :class="alertProtocol == true ? 'registrationInfo_alert_protocol' : 'no_registrationInfo_alert_protocol'">
			<view class="">
				<text>1、同意办理设立登记手续</text>
			</view>
			<view class="">
				<text>2、同意核对登记材料中的复印件并签署</text>
			</view>
			<view class="">
				<text>3、同意修改企业自备文件的错误</text>
			</view>
			<view class="">
				<text>4、同意修改有关表格的填写错误</text>
			</view>
			<view class="">
				<text>5、同意现场领取营业执照和有关文书</text>
			</view>
			<view class="">
				<text>6、同意邮寄营业执照和有关文书</text>
			</view>
			<view class="">
				<text>7、同意指定代表或委托代理人作为邮寄</text>
			</view>
			<view class="">
				<text>8、同意法定代表人（负责人、执行事务合伙人或委派代表）作为邮寄营业执照和有关文书的收件人</text>
			</view>
		</view>
		<view :class="alertProtocol == true ? 'registrationInfo_alert_img_x' : 'no_registrationInfo_alert_img_x'">
			<image class="registrationInfo_alert_img_x_item" @click="closeProtocol" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/error.png" mode=""></image>
		</view>
		<view :class="alertProtocol == true ? 'mengceng' : ''">
		</view>
		
		
		<view class="registrationInfo_header">
			<text>注册信息</text>
		</view>
		
		<view class="registrationInfo_info">
			<view class="registrationInfo_info_item">
				<view class="registrationInfo_info_item_left">
					<text>姓名</text>
				</view>
				<view class="registrationInfo_info_item_right">
					<text>{{realname}}</text>
				</view>
			</view>
			<view class="registrationInfo_info_item">
				<view class="registrationInfo_info_item_left">
					<text>身份证号</text>
				</view>
				<view class="registrationInfo_info_item_right">
					<text>{{idcard}}</text>
				</view>
			</view>
			<!-- 联系方式 -->
			<view class="registrationInfo_info_item">
				<view class="registrationInfo_info_item_left">
					<text>联系方式</text>
				</view>
				<view class="registrationInfo_info_item_right">
					<text>{{user_phone}}</text>
				</view>
			</view>
			<view class="registrationInfo_info_item">
				<view class="registrationInfo_info_item_left">
					<text>申请名称</text>
				</view>
				<view class="registrationInfo_info_item_right">
					<text>{{bussiness_name}}</text>
				</view>
			</view>
			<!-- 注册资本 -->
			<!-- <view class="registrationInfo_info_item">
				<view class="registrationInfo_info_item_left">
					<text>注册资本</text>
				</view>
				<view class="registrationInfo_info_item_right">
					<text>1 万元</text>
				</view>
			</view> -->
			<!-- 登记地址 -->
			<view class="registrationInfo_info_item">
				<view class="registrationInfo_info_item_left">
					<text>登记地址</text>
				</view>
				<view class="registrationInfo_info_item_right">
					<text>浙江省丽水市景宁畲族自治县红星街道108号</text>
				</view>
			</view>
			
			<!-- 登记机关 -->
			<view class="registrationInfo_info_item">
				<view class="registrationInfo_info_item_left">
					<text>登记机关</text>
				</view>
				<view class="registrationInfo_info_item_right">
					<text>景宁畲族自治县市场监督管理局</text>
				</view>
			</view>
			<!-- 管辖单位 -->
			<view class="registrationInfo_info_item">
				<view class="registrationInfo_info_item_left">
					<text>管辖单位</text>
				</view>
				<view class="registrationInfo_info_item_right">
					<text>景宁畲族自治县市场监督管理局</text>
				</view>
			</view>
			<!-- 经营范围 -->
			<view class="registrationInfo_info_item">
				<view class="registrationInfo_info_item_left">
					<text>经营范围</text>
				</view>
				<view class="registrationInfo_info_item_right">
					<text>网上销售：{{business_scope}}(依法须批准的醒目,经相关部门批准方可开展经营活动)</text>
				</view>
			</view>
			<!-- 主营业务 -->
			<view class="registrationInfo_info_item">
				<view class="registrationInfo_info_item_left">
					<text>主营业务</text>
				</view>
				<view class="registrationInfo_info_item_right">
					<text>{{main_scope}}</text>
				</view>
			</view>
			<!-- 执照预览 -->
			<view class="registrationInfo_info_item">
				<view class="registrationInfo_info_item_left">
					<text>执照预览</text>
				</view>
				<view class="registrationInfo_info_item_right">
					<text style="color: #4A90E2;" @click="lookPreImg">点击预览</text>
				</view>
			</view>
			
		</view>
		<!-- 
		<view class="registrationInfo_header" style="margin-top: 15px;">
			<text>个人信息</text>
		</view> -->
		
		
		<!-- <view class="registrationInfo_info">
			<view class="registrationInfo_info_item">
				<view class="registrationInfo_info_item_left">
					<text>姓名</text>
				</view>
				<view class="registrationInfo_info_item_right">
					<text>这个超人</text>
				</view>
			</view>
			<view class="registrationInfo_info_item">
				<view class="registrationInfo_info_item_left">
					<text>性别</text>
				</view>
				<view class="registrationInfo_info_item_right">
					<text>男</text>
				</view>
			</view>
				<view class="registrationInfo_info_item_left">
					<text>民族</text>
				</view>
				<view class="registrationInfo_info_item_right">
					<text>汉</text>
				</view>
			</view>
			<view class="registrationInfo_info_item">
				<view class="registrationInfo_info_item_left">
					<text>出生</text>
				</view>
				<view class="registrationInfo_info_item_right">
					<text>19999999</text>
				</view>
			</view>
			<view class="registrationInfo_info_item">
				<view class="registrationInfo_info_item_left">
					<text>身份证号</text>
				</view>
				<view class="registrationInfo_info_item_right">
					<text>310000000000000022</text>
				</view>
			</view>
			<view class="registrationInfo_info_item">
				<view class="registrationInfo_info_item_left">
					<text>地址</text>
				</view>
				<view class="registrationInfo_info_item_right">
					<text>地球美国帝国大厦顶楼</text>
				</view>
			</view>
			<view class="registrationInfo_info_item">
				<view class="registrationInfo_info_item_left">
					<text>签发机关</text>
				</view>
				<view class="registrationInfo_info_item_right">
					<text>复仇者联盟政审局</text>
				</view>
			</view>
			<view class="registrationInfo_info_item">
				<view class="registrationInfo_info_item_left">
					<text>有效期限</text>
				</view>
				<view class="registrationInfo_info_item_right">
					<text>2015.02.11-2015.02.29</text>
				</view>
			</view>
		</view> -->
		
		<!-- 同意协议 -->
		<view class="registrationInfo_protocol">
			<view class="registrationInfo_protocol_top">
					<radio value="A" @click="radioChange" :checked="radio" />
				 <text class="registrationInfo_protocol_top_txt" @click="lookProtocol">同意上海爱用宝科技股份有限公司的代办协议</text>
			</view>
			<view class="registrationInfo_protocol_bottom" @click="goMain_index">
				<button  size="mini" plain="true" style="width: 360upx; color: #1AAD19; border-color: #1AAD19;" >确认</button>
			</view>
		</view>
		
	</view>
</template>

<script>
	var _self;
	export default {
		data() {
			return {
				alertImg : false, //预览弹窗是否显示,默认不显示
				alertProtocol : false, //查看协议
				radio : false, 
				bussiness_name : '', // 企业名称
				realname : '', // 名字
				idcard : '', // 首份证号
				user_phone: '', // 用户手机号
				business_scope : '', // 经营范围
				main_scope : '' // 主营业务
				
			};
		},
		onLoad() {
			_self = this;
			uni.request({
				url: global.host + 'Zhu/getUserInfo',
				method: 'GET',
				data: {
					openid : global.openid
				},
				success: res => {
					console.log('查看该用户信息',res.data[0].user_phone);
					// 把用户信息存在缓存
					try {
						uni.setStorageSync('userinfo', res.data[0]);
					} catch (e) {
						console.log('设置用户信息失败');
						// error
					}
					let user_phone = res.data[0].user_phone;
					_self.user_phone = user_phone;
				},
				fail: () => {},
				complete: () => {}
			});
			try {
				let bussiness_name = uni.getStorageSync('bussiness_name');
				const infofront = uni.getStorageSync('infofront');
				const infoback = uni.getStorageSync('infoback');
				const business_scope = uni.getStorageSync('business_scope');
				console.log(business_scope);
				if (bussiness_name && infofront && infoback) {
					let {realname, idcard} =  infofront.data.result;
					this.realname = realname;
					this.idcard = idcard;
					let txt = '';
					try {
						const main_scope = uni.getStorageSync('main_scope');
						this.main_scope = main_scope;
						for(let i = 0; i < business_scope.length; i++ ){
							if(business_scope[i] != main_scope) {
								txt = business_scope[i] + ',' + txt;
							}
						}
						let text = main_scope + ',' + txt;
						try {
							uni.setStorageSync('business_scope_text', text.substr(0,text.length-1));
						} catch (e) {
							// error
						}
						this.business_scope = text.substr(0,text.length-1);
						this.bussiness_name = bussiness_name.replace(/名称查重通过/,'');
					} catch (e) {
						console.log('errormain_scope');
					}
					
				}
			} catch (e) {
				// error
			}	
		},
		methods:{
			lookPreImg() {
				this.alertImg = true;
				console.log('点击预览');
			},
			closeImgAlert() {
				this.alertImg = false;
				console.log('点击了关闭');
			},
			lookProtocol() {
				// this.alertProtocol = true;
				uni.navigateTo({
					url : '../protocol/protocol'
				});
				console.log('点击查看协议');
				
			},
			closeProtocol() {
				this.alertProtocol = false;
				console.log('点击关闭协议');
			},
			goMain_index() {
				if(this.radio == true) {
					// 修改step 并保留用户的身份证信息
					// 已经模拟身份证信息保存了
					try {
						const openid = uni.getStorageSync('openid');
						if (openid) {
							uni.request({
								url: global.host + 'Zhu/editCurrentStep',
								method: 'GET',
								data: {
									openid : openid,
									current_step : 3 // 3代表选择套餐阶段
								},
								success: res => {
									// 在跳转之前,要保留用户的信息
									uni.reLaunch({
										url: '../main_index/main_index?from=registrationInfo'
									});
// 									uni.redirectTo({
// 										url: '../main_index/main_index?from=registrationInfo'
// 									});
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
					uni.showToast({
					title: '请先同意筑商协议...',
					icon : 'none',
					});
				}
			
			},
			radioChange() {
				this.radio = !this.radio;
			}
		}
	}
</script>
<style>
.mengceng {
	background-color:rgba(151,151,151,0.8);
	width: 750upx;
	height: 100%;
	position: fixed;
	top: 0;
	z-index: 998;
}
	
.registrationInfo {
	margin: 0 auto;
	margin-top: 15px;
	text-align: center;
	margin-bottom: 15px;
	background: #9797979;
}

.registrationInfo_header {
	
}

.registrationInfo_info {
	margin: 0 auto;
	width: 674upx;
	/* height: 410px; */
	border: 1px solid #E5E5E5;
	margin-top: 10px;
}

.registrationInfo_info_item {
	/* padding-top: 10px; */
	/* padding-bottom: 10px; */
	padding-left: 15px;
	display: flex;
	flex-direction: row;
	border-bottom: 1px solid #E5E5E5;
	min-height: 41px;
}
.registrationInfo_info_item_left {
	display: flex;
	width: 170upx;
	font-size: 15px;
	color: #000000;
	font-weight: 600;
	align-items: center!important;
}
.registrationInfo_info_item_right {
	display: flex;
	align-items: center!important;
	text-align: left;
	color: #000000;
	font-size: 14px;
	width: 500upx;
}

.registrationInfo_protocol {
	margin-top: 50px;
	width: 666upx;
	height: 100px;
}
.registrationInfo_protocol_top {
	display: flex;
	flex-direction: row;
	color: #E77D53;
	justify-content: center;
}
.registrationInfo_protocol_top_radio {
	
}
.registrationInfo_protocol_top_radio {
	
}

.registrationInfo_protocol_bottom {
	margin-top: 20px;
}

.no_registrationInfo_alert_img {
	display: none;
}
.no_registrationInfo_alert_img_x {
	display: none;
}
.registrationInfo_alert_img {
	/* margin: 0 auto; */
	height: 500px;
	width: 700upx;
	background: yellow;
	z-index: 999;
	position: fixed;
	top: 15px;
	left: 13px;
	border-radius: 5px;
}
.registrationInfo_alert_img_x_item {
	width: 60upx;
	height: 30px;
	position: fixed;
	top: 540px;
	left: 330upx;
	z-index: 999;
}

/* 展示协议 */
.registrationInfo_alert_protocol {
	padding-top: 18px;
	padding-left: 19px;
	padding-right: 20px;
	text-align: left;
	height: 340px;
	width: 570upx;
	background: white;
	z-index: 999;
	position: fixed;
	top: 125px;
	left: 45px;
	box-sizing:border-box;
	border-radius: 5px;
}
.no_registrationInfo_alert_protocol {
	display: none;
}

</style>
