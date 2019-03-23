<template>
	<view class="name_repeat">
		
		<!-- 查看错误弹窗 -->
		<view :class="alertModal == true ? 'name_repeat_error_modal' : 'no_name_repeat_error_modal' ">
			<view class="name_repeat_error_title">
				<text>失败原因</text>
			</view>
			<view class="name_repeat_error_list">
				<text>1、{{error1}}</text>
				<!-- <text>2、失败原因二</text> -->
				<!-- <text>3、失败原因三</text> -->
				<!-- <text>4、失败原因四</text> -->
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
			<text class="name_repeat_header_title">景宁{{zihao}}{{array[0][indexzero]}}{{typeText}}</text>
		</view>
		<!-- 店铺起名字 -->
		<view class="name_repeat_set">
			<view :class="inputRed == true ? 'name_repeat_set_1 inputRed' : 'name_repeat_set_1'">
				<text>字号 :  </text>
				<input  @blur="zihaoBlur" type="text" value="" style="text-align: left; padding-left: 10upx;" placeholder="给店铺起个名字吧" />
			</view>
			<view class="name_repeat_header_type">
					<picker @change="bindPickerChange" :value="index" :range="array" mode="multiSelector" >
						<view class="" style="padding-left: 10upx; padding-right: 10upx;"><text style="color: #B2B2B2;">{{isFirst == true ? '点击选店铺后缀' : ''}}</text> 
						{{isFirst == true ? '' : array[0][indexzero]}}{{isFirst == true ? '' : array[1][index]}}</view>
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
				// array : [['a','b'],['网店', '店','铺', '商行', '工作室','商店','服务部','事务所']],
				array : [['a','b'],['店', '网店']],
				index: 0, // 形式
				indexzero: 0, // 主营业务
				canClick : false, // 开始查询 是否能够点击
				inputRed : false, // 字号自我检测有问题的时候标红
				zihao : '(字号)',
				type : '网店',
				typeText : '网店',
				industry_description : '',
				isFirst : true, //是否第一次进来,是的话,需要重新选择 店铺后缀
				error1 : '', // 第一道字号检测错误一
			};
		},
		onLoad() {
			// 查重之前先清理库内过期的字号
			var self = this;
			try {
				const business_scope = uni.getStorageSync('business_scope');
				if (business_scope) {
					console.log(business_scope);
					self.array[0] = business_scope;
					if(business_scope[0] == '健康销售') {
						self.array[1] = ['店'];
					}
				}
			} catch (e) {
				// error
			}
			console.log(888);
			uni.request({
				url:  global.host + 'Zhu/clearCharacterTimeout',
				method: 'GET',
				data: {},
				success: res => {
					console.log('清理过期的字号',res);
				},
				fail: () => {},
				complete: () => {}
			});
			console.log(666);
			// 首先需要加载经营范围
// 			try {
// 				const value = uni.getStorageSync('business_scope');
// 				console.log('business_scope',business_scope);
// 				if (value) {
// 					console.log('111',value);
// 					self.array[0] = value;
// 				}
// 			} catch (e) {
// 				// error
// 			}
		},
		methods: {
			change() {
				console.log(11);
			},
			bindPickerChange: function(e) {
				this.isFirst = false; // 改变以后,  文字 请选择店铺后缀就不会出现
				console.log('picker发送选择改变，携带值为', e.target.value)
				this.index = e.target.value[1];
				this.indexzero = e.target.value[0];
				
				if(e.target.value[1] == 0) {
					this.type = 3;
					this.typeText = '店';
				}
				if(e.target.value[1] == 1) {
					this.type = 32;
					this.typeText = '网店';
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
				// 将主营业务缓存
				try {
					uni.setStorageSync('main_scope',this.array[0][e.target.value[0]]);
				} catch (e) {
					// error
				}
			},
			startfind() {
				
				uni.request({
					url: global.host + 'Zhu/getController',
					method: 'GET',
					data: {},
					success: res => {
						// 点击查重 做一个开关
						console.log('查看开关情况',res);
						if(res.data[0].switch11 != 1) {
							// 已经被关闭 不能进行查重
							// 在维护
							uni.showLoading({
								title: '第一批邀请测试结束，敬请期待下一次开放。'
							});
							setTimeout(()=>{
								  uni.hideLoading();
							},2000)
						} else {
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
								try {
									uni.setStorageSync('zihao', this.zihao);
								} catch (e) {
									// error
									console.log('设置字号失败');
								}
								let name = encodeURIComponent(this.zihao).replace(/25+/g,'');
								let business = encodeURIComponent(this.industry_description).replace(/25+/g,'');
								uni.redirectTo({
									url: '../name_repeat_ing/name_repeat_ing?character=' +name + '&business=' + business +'&business_big=' + returnInfo[0] + '&business_center='+ returnInfo[1] + '&business_small=' + returnInfo[2] + '&organization=' + this.type
								});
							} else {
								if(this.inputRed) {
									uni.showToast({
										title: '请修改字号',
										duration: 2000,
										icon :'none'
									});
								} else {
									uni.showToast({
										title: '请先填写字号',
										duration: 2000,
										icon :'none'
									});
								}
							}
						}
					},
					fail: () => {},
					complete: () => {}
				});
				
			
			},
			zihaoBlur(e) {
				console.log('失去焦点',e.detail.value);
				if(e.detail.value != '') {
					// 不等于空 可以点击开始查询
					uni.showLoading({
						title: '处理中'
					});
					
					uni.request({
						url:  global.host + 'Zhu/verificationContent?content=' + e.detail.value,
						method: 'GET',
						data: {},
						success: res => {
							console.log(res);
							if(res.data.result.spam == 0) {
								console.log('global.verificationContent(e.detail.value)',global.verificationContent(e.detail.value));
								if(global.verificationContent(e.detail.value) != 'ok') {
									this.error1 = global.verificationContent(e.detail.value);
									this.alertModal = true;
									this.canClick = false;
									this.inputRed = true;
									uni.hideLoading();
								} else {
									this.error1 = global.verificationContent(e.detail.value);
									this.zihao = e.detail.value;
									this.inputRed = false;
									this.canClick = true;
									uni.hideLoading();
								}
							} else {
								this.error1 = '不能含有中外地名、国际组织、中外历史名人或者公司品牌名称以及敏感词';
								this.alertModal = true;
								this.canClick = false;
								this.inputRed = true;
								uni.hideLoading();
							}
						},
						fail: () => {},
						complete: () => {}
					});
					
					
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
.name_repeat_error_title {
	margin-top: 20px;
}
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
	width: 260upx;
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
