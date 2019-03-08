<template>
	<view class="main_content">
		<view class="main_header">
		</view>
		
		<!-- 头像 姓名 -->
		<view class="main_avator_name">
			<!-- 头像 -->
			<image class="main_avator" src="https://static.dingtalk.com/media/lADPDgQ9qUSQq5zNBLbNBNc_1239_1206.jpg" mode=""></image>
			<!-- 姓名 -->
			<view class="main_name">
				<text>这个彬</text>
			</view>
		</view>
		
		<!-- 主页-介绍 -->
		<view class="main_title">
			<text>
				简单5步在线申请个体营业执照
			</text>
		</view>
		
		<!-- 进度状态 -->
		<view class="main_progress">
			<view class="left_progress">
				<!-- 选择行业 -->
				<view class="" style="position: relative;">
					<image :class="currentStep == 0 ? 'current_progress' : 'green_ok'" style="position: absolute; top: -5px; left: -12upx;" 
					:src="currentStep == 0 ? 'http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/current_progress.png' : (currentStep > 0 ? 'http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/green_ok.png' : 'http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/gray_spot.png')" mode=""></image>
					<view class="spindle1">
					</view>
				</view>
				<!-- 名称查重 -->
				<view class="" style="position: relative;">
					<image :class="currentStep == 1 ? 'current_progress' : 'green_ok'"  style="position: absolute; top: -3px; left: -12upx;" 
					:src="currentStep == 1 ? 'http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/current_progress.png' : (currentStep > 1 ? 'http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/green_ok.png' : 'http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/gray_spot.png')"mode=""></image>
					<view class="spindle2">
						
					</view>
				</view>	
				<!-- 注册登记 -->
				<view class="" style="position: relative;">
					<image :class="currentStep == 2 ? 'current_progress' : 'green_ok'" style="position: absolute; top: -3px; left: -12upx;" 
					:src="currentStep == 2 ? 'http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/current_progress.png' : (currentStep > 2 ? 'http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/green_ok.png' : 'http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/gray_spot.png')" mode=""></image>
					<view class="spindle3">
						
					</view>
				</view>
				<!-- 选择套餐并完成支付 -->
				<view class="" style="position: relative;">
					<image :class="currentStep == 3 ? 'current_progress' : 'green_ok'" style="position: absolute; top: -3px; left: -12upx;"
					:src="currentStep == 3 ? 'http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/current_progress.png' : (currentStep > 3 ? 'http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/green_ok.png' : 'http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/gray_spot.png')" mode=""></image>
					<view class="spindle4">
						
					</view>
				</view>
				<!-- 审核通过并领取材料 -->
				<view class="" style="position: relative;">
					<image :class="currentStep == 4 ? 'current_progress' : 'green_ok'" style="position: absolute; top: -3px; left: -12upx;" 
					:src="currentStep == 4 ? 'http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/current_progress.png' : (currentStep > 4 ? 'http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/green_ok.png' : 'http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/gray_spot.png')" mode=""></image>
				</view>
			</view>
			<view class="right_progress">
				<view class="right_progress_item">
					<view class="">
						<text>选择行业</text> <text :class="currentStep > 0 ? 'progress_status' : 'progress_status_gray'">{{currentStep > 0 ? '(已完成)' : '(待完成)'}}</text>
					</view>
					<view class="step_detail">
						<text>选择你从事的行业,用以确定经营范围</text>
					</view>
				</view>
				<view class="right_progress_item">
					<view class="">
						<text>名称查重</text> <text :class="currentStep > 0 ? 'progress_status' : 'progress_status_gray'">{{currentStep  > 1? '(已完成)' : '(待完成)'}}</text>
					</view>
					<view class="step_detail">
						<text>给你的店铺取个合适的名字</text>
					</view>
				</view>
				<view class="right_progress_item">
					<view class="">
						<text>注册登记</text> <text :class="currentStep > 0 ? 'progress_status' : 'progress_status_gray'">{{currentStep > 2 ? '(已完成)' : '(待完成)'}}</text>
					</view>
					<view class="step_detail">
						<text>填写登记信息并完成实名认证</text>
					</view>
				</view>
				<view class="right_progress_item">
					<view class="">
						<text>选择套餐并完成支付</text> <text :class="currentStep > 0 ? 'progress_status' : 'progress_status_gray'">{{currentStep > 3 ? '(已完成)' : '(待完成)'}}</text>
					</view>
					<view class="step_detail">
						<text>根据自己的实际情况选择合适的套餐</text>
					</view>
				</view>
				<view class="right_progress_item">
					<view class="">
						<text>审核通过并领取材料</text> <text :class="currentStep > 0 ? 'progress_status' : 'progress_status_gray'">{{currentStep > 4 ? '(已完成)' : '(待完成)'}}</text>
					</view>
					<view class="step_detail">
						<text>快递寄送公章,税盘等材料</text>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 名称查重btn -->
		<view class="main_find_repeat">
			<button type="" @click="clickStep" size="mini" plain="true" style="width: 360upx; color: #1AAD19; border-color: #1AAD19;" hover-class = "btn_hover">
				{{stepName}}
			</button>
		</view>
		
		<!-- 常见问题 -->
		<view class="">
			<text class="commonProblem" @click="reApply">
				重新申请
			</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// stepName : '名称查重',
				// stepName : '注册登记',
				// stepName : '选择套餐',
				// stepName : '查看进度',
				// stepName : '更多服务',
				stepName : '选择行业',
				currentStep : 0, // 默认0 为选择行业
			};
		},
		onLoad(e) {
			var self = this;
			uni.getStorage({
				key: 'currentStep',
				success: function (res) {
					console.log('storage',res.data);
					// self.currentStep = 1;
				}
			});
			console.log('主页打印onload的e',e);
			switch (e.from){
				case 'choiceIndustryOne':
					this.stepName = '名称查重';
					this.currentStep = 1;
					uni.setStorage({
						key: 'currentStep',
						data: '1',
						success: function () {
							console.log('success');
						}
					});
					break;
				case 'choiceInvitationCode':
					this.stepName = '名称查重';
					this.currentStep = 1;
					uni.setStorage({
						key: 'currentStep',
						data: '1',
						success: function () {
							console.log('success');
						}
					});
					break;
				case 'name_repeat':
					this.stepName = '注册登记';
					this.currentStep = 2;
					uni.setStorage({
						key: 'currentStep',
						data: '2',
						success: function () {
							console.log('success');
						}
					});
					break;
				case 'registrationInfo':
					this.stepName = '选择套餐';
					this.currentStep = 3;
					uni.setStorage({
						key: 'currentStep',
						data: '3',
						success: function () {
							console.log('success');
						}
					});
					break;
				case 'waitPay':
					this.stepName = '查看进度';
					this.currentStep = 4;
					uni.setStorage({
						key: 'currentStep',
						data: '4',
						success: function () {
							console.log('success');
						}
					});
					break;
				case 'review':
					this.stepName = '查看进度';
					this.currentStep = 4;
					uni.setStorage({
						key: 'currentStep',
						data: '4',
						success: function () {
							console.log('success');
						}
					});
					break;
				default:
					break;
			}
		},
		methods: {
			clickStep() {
				switch (this.stepName){
					case '名称查重':
						uni.redirectTo({
							url: '../name_repeat/name_repeat'
						});
						break;
					case '注册登记':
						uni.redirectTo({
							url: '../registration/registration'
						});
						break;
					case '选择套餐':
						uni.redirectTo({
							url: '../waitPay/waitPay'
						});
						break;
					case '查看进度':
						uni.redirectTo({
							url: '../review/review'
						});
						break;
					case '更多服务':
						uni.redirectTo({
							url: '../choiceIndustry/choiceIndustry'
						});
						break;
					case '选择行业':
						uni.redirectTo({
							url: '../choiceIndustry/choiceIndustry'
						});
						break;
						
					default:
						break;
				}
			},
			reApply() {
				uni.showModal({
				title: '提示',
				content: '重新申请将会清空完成的所有步骤',
				success: function (res) {
					if (res.confirm) {
						console.log('用户点击确定');
					} else if (res.cancel) {
						console.log('用户点击取消');
					}
				}
			});
			}
		}
	}
</script>

<style>
	.btn_hover {
		color: black!important;
	}
	.main_content {
		text-align: center;
	}
	.main_header {
		border-bottom-left-radius: 7px;
		border-bottom-right-radius: 7px;
		background: #E77D53;
		height: 60px;
	}
	
	.main_avator_name {
		position: absolute;
		top: 20px;
		left: 20upx;
		align-items: center;
		box-sizing:border-box;
		padding-left: 30upx;
		background: white;
		width: 710upx;
		height: 72px;
		border:1px solid #E77D53;
		display: flex;
		flex-direction: row;
		border-radius: 10px;
	}
	.main_avator {
		width: 84upx;
		height: 42px;
		border-radius: 50%;
	}
	.main_name {
		margin-left: 30upx;
		align-items: center;
	}
	
	.main_title {
		margin-top: 60px;
		font-size: 17px;
		font-style: normal;
	}
	/* 进度状态部分 */
	.main_progress {
		font-style: normal;
		display: flex;
		margin: 0 auto;
		width: 539upx;
		height: 400px;
		/* background: gray; */
		margin-top: 40px;
	}
	.left_progress {
		position: relative;
		padding-left: 50upx;
		padding-top: 10px;
		width: 100upx;
		color: #333333;
		font-size: 14px;
	}
	.right_progress {
		text-align: left;
		flex-direction: column;
		width: 440upx;
		height: 43px;
	}
	.step_detail {
		padding-left: 32upx;
		color: #999999;
		font-size: 12px;
	}
	.progress_status {
		color: #1AAD19;
		font-size: 12px;
		margin-left: 18upx;
	}
	.progress_status_gray {
		color: #999999;
		font-size: 12px;
		margin-left: 18upx;
	}
	.right_progress_item {
		margin-bottom: 21px;
	}
	
	/* 常见问题 */
	.commonProblem {
		/* color: #BD10E0; */
		font-size: 10px;
	}
	
	/* 进度杠子 */
	.spindle1 {
		background: #1AAD19;
		height: 68px;
		width: 4upx;
	}
	.spindle2 {
		height: 68px;
		width: 4upx;
		background: #DDDDDD;
	}
	.spindle3 {
		height: 68px;
		width: 4upx;
		background: #DDDDDD;
	}
	.spindle4 {
		height: 68px;
		width: 4upx;
		background: #DDDDDD;
	}
	.green_ok {
		width: 30upx;
		height: 15px;
	}
	.current_progress {
		left: -15px!important;
		width: 60upx;
		height: 30px;
	}
	.gray_spot {
		width: 30upx;
		height: 15px;
	}
</style>
