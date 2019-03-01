<!-- 注册登记  基本信息 -->
<template>
	<view class="registration">
		<view class="registration_name">
			<text>基本信息</text>
		</view>
		<!-- 注册资金 手机号 -->
		<view :class="isDisplayVerificationCode == true ? 'registration_block_add' : 'registration_block'">
			<view class="registration_block_1">
				<text>注册资金</text>  
				<picker @change="bindPickerChange" :value="index" :range="array">
					<view class="" style=" padding-left: 10upx; padding-right: 10upx;"><text style="display: inline-block; width: 50upx; border-bottom: 1px solid #E5E5E5; margin-left: 5upx; margin-right: 5upx;">{{array[index]}}</text>万元</view>
					<!-- <image src="" mode=""></image> -->
				</picker>
			</view>
			<view class="registration_block_2">
				手机号 <input type="text"   :value="phone" /> <text @click="changePhone" :class="isDisplayVerificationCode == true ? 'verificationCodeStyle' : ''" style="color: #4A90E2;">{{phoneTxt}}</text> 
			</view>
			<view :class="isDisplayVerificationCode == true ? 'registration_block_3':'no_registration_block_3'">
				验证码 <input type="text"   :value="verificationCode" /> <text @click="" style="color: #FF0000;">验证码错误</text>
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
			<view class="registration_ok_left">
				<image class="registration_ok_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/pre_step.png" mode=""></image>
				<text class="registration_ok_txt">重新名称查重</text>
			</view>
			<view class="registration_ok_right">
				<image class="registration_ok_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/right_icon.png" mode=""></image>
				<text class="registration_ok_txt">信息确定</text>
			</view>
			
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				phone : 17637794541,
				verificationCode : 666666,
				array: ['1', '10', '100'],
				index: 0,
				isDisplayVerificationCode : false, //是否显示验证码这一栏,默认不显示
				phoneTxt : '修改',
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
						console.log(JSON.stringify(res.tempFilePaths));
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
			changePhone() {
				// 修改手机号,要显示验证码这一栏
				this.isDisplayVerificationCode = true;
				this.phoneTxt = '获取验证码';
				console.log('修改号码');
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
	height: 84px;
	border: 1px solid #E5E5E5;
	border-radius: 5px;
}
.registration_block_add {
	margin: 0 auto;
	width: 674upx;
	height: 132px;
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
