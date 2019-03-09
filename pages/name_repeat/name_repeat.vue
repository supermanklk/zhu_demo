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
			<text class="name_repeat_header_title">景宁  {{zihao}}  {{typeText}}</text>
		</view>
		<!-- 店铺起名字 -->
		<view class="name_repeat_set">
			<view :class="inputRed == true ? 'name_repeat_set_1 inputRed' : 'name_repeat_set_1'">
				<text>字号:</text>
				<input  @blur="zihaoBlur" type="text" value="" placeholder="给店铺起个名字吧" />
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
				array : [['a','b'],['网店', '店','铺', '商行', '工作室','商店','服务部','事务所']],
				index: 0,
				canClick : false, // 开始查询 是否能够点击
				inputRed : false, // 字号自我检测有问题的时候标红
				zihao : '(字号)',
				type : '网店',
				typeText : '网店',
				industry_description : ''
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
				if(e.target.value[1] == 0) {
					this.type = 32;
					this.typeText = '网店';
				}
				if(e.target.value[1] == 1) {
					this.type = 3;
					this.typeText = '店';
				}
				if(e.target.value[1] == 2) {
					this.type = 8;
					this.typeText = '铺';
				}
				if(e.target.value[1] == 3) {
					this.type = 18;
					this.typeText = '商行';
				}
				if(e.target.value[1] == 4) {
					this.type = 29;
					this.typeText = '工作室';
				}
				if(e.target.value[1] == 5) {
					this.type = 53;
					this.typeText = '商店';
				}
				if(e.target.value[1] == 6) {
					this.type = 42;
					this.typeText = '服务部';
				}
				if(e.target.value[1] == 7) {
					this.type = 58;
					this.typeText = '事务所';
				}
				this.industry_description = this.array[0][e.target.value[0]];
				console.log(this.array[0][e.target.value[0]]);
			},
			startfind() {
				// let returnInfo =  global.returnData('服装');
				let returnInfo =  global.returnData(this.industry_description);
				if(this.industry_description == '') {
					uni.showToast({
						title: '请选择经营范围',
						duration: 2000,
						icon :'none'
					});
					return false;
				}
				if(this.canClick) {
					//开始查重 目前是随机的 没有真实的数据
// 					let num = Math.random();
// 					if(num > 0.5) {
// 						// 查重失败的界面
// 						uni.navigateTo({
// 							url: '../nameRepeatError/nameRepeatError'
// 						});	
// 					} else {
// 						// 查重成功的界面
// 						uni.navigateTo({
// 							url: '../nameRepeatSuccess/nameRepeatSuccess'
// 						});	
// 					}
					uni.showLoading({
						title: '正在查重,请耐心等待'
					});
					let name = encodeURIComponent(this.zihao).replace(/25+/g,'');
					let business = encodeURIComponent(this.industry_description).replace(/25+/g,'');
					uni.request({
						url: global.host + 'Zhu/getName?character='+name + '&business=' + business,
						method: 'GET',
						data: {
							business_big : returnInfo[0],
							business_center : returnInfo[1],
							business_small : returnInfo[2],
							organization : this.type,
							account : 'supermanzhangbin',
							password : 'Zhangbin521..'
						},
						success: res => {
							console.log('325435',res);
							uni.hideLoading();
							if(res.data) {
								if(res.data.indexOf('查重失败') != -1) {
									// 说明查重失败
									uni.navigateTo({
										url: '../nameRepeatError/nameRepeatError'
									});	
								}else if(res.data.indexOf('查重通过') != -1) {
									// 查询通过,把这个字号給它缓存到本地
									try {
										uni.setStorageSync('bussiness_name', res.data.split('，')[0]);
									} catch (e) {
										// error
									}
									uni.navigateTo({
										url: '../nameRepeatSuccess/nameRepeatSuccess?name=' + name + '&business=' + business + '&organization=' + this.type
									});	
								}
							}
						},
						fail: (e) => {
							console.log('查重报错',e);
							uni.hideLoading();
						},
						complete: () => {}
					});
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
						this.zihao = e.detail.value;
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
	width: 236upx;
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
