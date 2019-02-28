<template>
	<view class="name_repeat">
		<view class="name_repeat_header">
			<image class="name_repeat_header_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/office_icon.png" mode=""></image>
			<text class="name_repeat_header_title">景宁  (字号)  服装店</text>
		</view>
		<!-- 店铺起名字 -->
		<view class="name_repeat_set">
			<view class="name_repeat_set_1">
				<text>字号:</text>
				<input type="text" value="" placeholder="给店铺起个名字" />
			</view>
			<view class="name_repeat_header_type">
					<picker @change="bindPickerChange" :value="index" :range="array">
						<view class="" style="padding-left: 10upx; padding-right: 10upx;">{{array[index]}}</view>
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
		<view class="choiceIndustry_pre_nex" style="margin-top: 60px;">
			<view class="choiceIndustry_pre_nex_left">
				<image class="choiceIndustry_pre_nex_left_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/left_right.png" mode=""></image>
				<text class="choiceIndustry_pre_nex_left_txt">重新选择行业</text>
			</view>
			<view class="choiceIndustry_pre_nex_right">
				<image class="choiceIndustry_pre_nex_right_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/right_icon.png" mode=""></image>
				<text class="choiceIndustry_pre_nex_right_txt">开始查询</text>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
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
				array: ['店', '铺', '室', '工作室','经营部','服务部','事务所'],
				index: 0,
			};
		},
		methods: {
			change() {
				console.log(11);
			},
			bindPickerChange: function(e) {
				console.log('picker发送选择改变，携带值为', e.target.value)
				this.index = e.target.value
			},
		}
	}
</script>

<style>
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


</style>
