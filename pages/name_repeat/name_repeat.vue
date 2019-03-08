<template>
	<view class="name_repeat">
		
		<!-- 查看错误弹窗 -->
		<view :class="alertModal == true ? 'name_repeat_error_modal' : 'no_name_repeat_error_modal' ">
			<view class="name_repeat_error_title">
				<text>失败原因</text>
			</view>
			<view class="name_repeat_error_list">
				<text>1、名字不能含有彬</text>
				<text>2、失败原因二</text>
				<text>3、失败原因三</text>
				<text>4、失败原因四</text>
			</view>
		</view>
		<!-- 弹窗关闭的X -->
		<view :class="alertModal == true ? 'name_repeat_error_modal_X' : 'no_name_repeat_error_modal_X' ">
			<image class="alert_img_x_item" @click="closeImgAlert" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/error.png" mode="" ></image>
		</view>
		<!-- 蒙层 -->
		<view :class="alertModal == true ? 'mengceng' : ''">
		</view>
		
		<view class="name_repeat_header">
			<image class="name_repeat_header_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/office_icon.png" mode=""></image>
			<text class="name_repeat_header_title">景宁  (字号)  服装店</text>
		</view>
		<!-- 店铺起名字 -->
		<view class="name_repeat_set">
			<view :class="inputRed == true ? 'name_repeat_set_1 inputRed' : 'name_repeat_set_1'">
				<text>字号:</text>
				<input  @blur="zihaoBlur" type="text" value="" placeholder="给店铺起个名字" />
			</view>
			<view class="name_repeat_header_type">
					<picker @change="bindPickerChange" :value="index" :range="array" mode="multiSelector">
						<view class="" style="padding-left: 10upx; padding-right: 10upx;">{{array[1][index]}}</view>
						<!-- <image src="" mode=""></image> -->
					</picker>
			</view>
		</view>
		
		<!-- 推荐列表 -->
		<view class="name_repeat_list">
			<image class="name_repeat_list_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/recommend.png" mode=""></image>
			<view class="" v-for="(item,index) in list" :key="index">
				<view class="list_item">
					{{item.name}}
				</view>
			</view>
		</view>
		
		<!-- 换一批 -->
		<view class="changeBatch">
			<text class="changeBatch_txt">换一批</text>
			<image class="changeBatch_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/changeBatch.png"></image>
		</view>
		
		<!-- 名称查重btn -->
		<!-- <view class="">
			<button type="" size="mini" plain="true" style="margin-top: 74px; width: 360upx; color: #1AAD19; border-color: #1AAD19;" hover-class = "btn_hover">
				查询
			</button>
		</view> -->
		
		<!-- 底部重新查询 注册登记 -->
		<!-- <view class="choiceIndustry_pre_nex" style="margin-top: 60px;">
			<view class="choiceIndustry_pre_nex_right" @click="startfind">
				<image class="choiceIndustry_pre_nex_right_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/right_icon.png" mode=""></image>
				<text class="choiceIndustry_pre_nex_right_txt">开始查询</text>
			</view>
		</view> -->
		
		<!-- 开始查询 -->
		<view class="pay_btn">
			<button @click="startfind" type="" size="mini" plain="true" style="margin-top: 20px; width: 360upx; color: #1AAD19; border-color: #1AAD19;" hover-class = "btn_hover">
				开始查询
			</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				alertModal : false, // 查看失败原因弹窗,
				list : [
					{ name : '推荐名称一'},
					{ name : '推荐名称二'},
					{ name : '推荐名称三'},
					{ name : '推荐名称四'},
					{ name : '推荐名称五'}
				],
				range : [
					'1','2','3'
				],
				array : [['a','b'],['网店', '店','铺', '室', '工作室','经营部','服务部','事务所']],
				index: 0,
				canClick : false, // 开始查询 是否能够点击
				inputRed : false, // 字号自我检测有问题的时候标红
			};
		},
		onLoad() {
			var self = this;
			// 首先需要加载经营范围
			try {
				const value = uni.getStorageSync('business_scope');
				if (value) {
					// console.log('111',value);
					self.array[0] = value;
				}
			} catch (e) {
				// error
			}
		},
		methods: {
			change() {
				console.log(11);
			},
			bindPickerChange: function(e) {
				console.log('picker发送选择改变，携带值为', e.target.value)
				this.index = e.target.value[1];
			},
			startfind() {
				if(this.canClick) {
					//开始查重 目前是随机的 没有真实的数据
					let num = Math.random();
					if(num > 0.5) {
						// 查重失败的界面
						uni.navigateTo({
							url: '../nameRepeatError/nameRepeatError'
						});	
					} else {
						// 查重成功的界面
						uni.navigateTo({
							url: '../nameRepeatSuccess/nameRepeatSuccess'
						});	
					}
				} else {
					uni.showToast({
						title: '请先填写字号',
						duration: 2000,
						icon :'none'
					});
				}
				
			},
			zihaoBlur(e) {
				console.log('失去焦点',e.detail.value);
				if(e.detail.value != '') {
					// 不等于空 可以点击开始查询
					uni.showLoading({
						title: '处理中'
					});
					if(e.detail.value.indexOf('彬') != -1) {
						this.alertModal = true;
						this.canClick = false;
						this.inputRed = true;
						uni.hideLoading();
					} else {
						this.inputRed = false;
						this.canClick = true;
						uni.hideLoading();
					}
				} else {
					this.canClick = false;
				}
				// 这里需要字号的违规检测
			},
			closeImgAlert() {
				this.alertModal = false;
			},
		}
	}
</script>

<style>
.inputRed {
	border-color: red!important;
}
.name_repeat_error_list {
	text-align: left;
	margin-top: 10px;
	display: flex;
	flex-direction: column;
	margin-left: 19px;
}
/* 弹窗样式 */
.name_repeat_error_modal {
	height: 310px;
	width: 570upx;
	background-color: white;
	z-index: 999;
	position: fixed;
	top: 125px;
	left: 45px;
	border-radius: 5px;
}
.no_name_repeat_error_modal {
	display: none;
}
/* 关闭的X */
.name_repeat_error_modal_X {
	width: 60upx;
	height: 30px;
	position: fixed;
	z-index: 999;
	top: 450px;
	left: 172px;
}
.no_name_repeat_error_modal_X {
	display: none;
}
/* 蒙层 */
.mengceng {
	background-color:rgba(151,151,151,0.8);
	width: 750upx;
	height: 100%;
	position: fixed;
	top: 0;
	z-index: 998;
}
.alert_img_x_item {
	width: 60upx;
	height: 30px;
	position: fixed;
	top: 450px;
	left: 330upx;
	z-index: 999;
}

	
.name_repeat_set {
	margin-top: 35px!important;
}
.name_repeat_set_1 {
	padding-left: 20upx;
	border-radius: 5px;
	border: 1px solid #E5E5E5;
	width: 500upx;
	height: 32px;
	display: flex;
	flex-direction: row;
	align-items: center;
	margin-right: 10upx;
}
.name_repeat_header_type {
	width: 144upx;
	height: 32px;
	border: 1px solid #E5E5E5;
	display: flex;
	flex-direction: row;
	line-height: 32px;
	border-radius: 5px;
	align-items: center;
	padding-left: 20upx;
}
.name_repeat_list_img {
	position: absolute;
	top: 0;
	right: 0upx;
	width: 200upx;
	height: 100px;
}
.name_repeat {
	text-align: center;
}
.name_repeat_header {
	flex-direction: column;
}
.name_repeat_header_img {
	display: flex;
	margin: 0  auto;
	margin-top: 35px;
	margin-bottom: 15px;
	width: 80upx;
	height: 40px;
}
.name_repeat_header_title {
	margin-top: 15px;
	font-size: 14px;
	font-style: normal;
}

.name_repeat_set {
	margin: 0 auto;
	width: 700upx;
	display: flex;
	flex-direction: row;
}
.name_repeat_list {
	position: relative;
	border-radius: 5px;
	border: 1px solid #E5E5E5;
	margin: 0 auto;
	margin-top: 35px;
	width: 700upx;
}

.list_item {
	font-size: 14px;
	/* padding: 10px auto 10px 16px; */
	padding-top: 10px;
	padding-bottom: 10px;
	padding-left: 16px;
	text-align: left;
	border-bottom: 1px solid #E5E5E5;
}

.changeBatch {
	margin-top: 20px;
	justify-content: center;
	display: flex;
	flex-direction: row;
	color: #E77D53;
}
.changeBatch_txt {
	font-style: normal;
	font-size: 14px;
}
.changeBatch_img {
	margin-left: 10upx;
	width: 40upx;
	height: 20px;
}
.btn_hover {
	color: black!important;
}
.pay_btn {
	margin: 0 auto;
	text-align: center;
	margin-top: 30px;
}


</style>
