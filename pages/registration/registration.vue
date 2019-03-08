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
				<image class="id_block_add" @click="clickFront" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/add.png" mode=""></image>
				<text>身份证人像面</text>
				<image class="card_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/id_front_img.png" mode=""></image>
			</view>
			<view class="id_block">
				<image class="id_block_add" @click="clickBack" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/add.png" mode=""></image>
				<text>身份证国徽面</text>
				<image  class="card_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/id_back_img.png" mode=""></image>
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
	export default {
		data() {
			return {
				phone : 17637794541,
				verificationCode : '',
				array: ['1', '10', '100'],
				index: 0,
				isDisplayVerificationCode : false, //是否显示验证码这一栏,默认不显示
				phoneTxt : '修改',
				codeTime: '', //默认信息反应的时间为60s
				waitCode : '', //给用户发送的短信验证码
				vertifyTxt : '',
			};
		},
		methods: {
			clickFront() {
				console.log('点击了正面');
				uni.chooseImage({
					count: 1, //默认9
					sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
					// sourceType: ['album'], //从相册选择
					success: function (res) {
						console.log(JSON.stringify(res));
						var tempFilePaths = res.tempFilePaths;
						 uni.saveFile({
						  tempFilePath: tempFilePaths[0],
						  success: function (res) {
							  console.log('保存返回的数据',res);
							var savedFilePath = res;
						  }
						});
					}
				});
			},
			clickBack() {
				console.log('点击了反面');
				uni.chooseImage({
					count: 1, //默认9
					sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
					// sourceType: ['album'], //从相册选择
					success: function (res) {
						console.log(JSON.stringify(res.tempFilePaths));
					}
				});
			},
			bindPickerChange: function(e) {
				console.log('picker发送选择改变，携带值为', e.target.value)
				this.index = e.target.value
			},
			changePhone(e) {
				// 判断点击的是 修改 还是 获取验证码
				if(this.phoneTxt == '修改') {
					// 修改手机号,要显示验证码这一栏
					this.isDisplayVerificationCode = true;
					this.phoneTxt = '获取验证码';
					console.log('修改号码');
				} else if(this.phoneTxt == '获取验证码' || this.phoneTxt == '重新获取验证码') {
					
					// 需要给用户的手机号发送短信
					let code = global.getVeritifyCode(); // 这个函数在全局
					let mobile = this.phone; //将要绑定的手机号,即发送的手机号
					let tpl_id = 140513; //模板号 模板就是发送信息的模板,模板是聚合数据的模板
					let tpl_value = '%23code%23%3D' + code;
					let key = '8972e94284e4fb4fafc3266c8834d25c'; //聚合数据的appkey
					let url = "http://v.juhe.cn/sms/send?mobile=" + mobile + '&tpl_id=' + tpl_id + '&dtype=&key=' +  key + '&tpl_value=' + tpl_value; 
					uni.request({
						url: url, 
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
				// 判断用户输入的验证码与发送的验证码是否一致
				if(e.detail.value == this.waitCode && e.detail.value !='') {
					this.vertifyTxt = '验证成功';
					this.codeTime = 0;
					// this.phoneTxt = '重新取验证码';
					uni.showToast({
						title: '修改成功了',
						duration: 2000
					});
					setTimeout(() => {
						this.isDisplayVerificationCode  = false;
						this.phoneTxt = '修改';
						this.vertifyTxt = '';
						this.verificationCode = '';
					},2000)
				} else if(this.phoneTxt == '获取验证码'){
					uni.showToast({
						title: '请先获取验证码',
						duration: 2000,
						icon :'none'
					});
				} else {
					this.vertifyTxt = '验证码错误';
				}
			},
			goRegistrationInfo() {
				uni.navigateTo({
					url: '../registrationInfo/registrationInfo'
				});
			},
			vertifyPhone(e) {
				let phone = e.detail.value;
				if(phone.length != 11) {
					uni.showToast({
						title: '手机号不正确',
						duration: 2000,
						icon :'none'
					});
				} else {
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
	margin-left: 50upx;
	height: 42px;
	line-height: 42px;
	display: flex;
	flex-direction: row;
	align-items: center;
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
