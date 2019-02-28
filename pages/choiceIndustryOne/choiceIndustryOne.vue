<!-- 选择行业-选择一个作为主营业务 -->
<template>
	<view class="choiceIndustryOne">
		<!-- 选择一个作为主营业务 -->
		<view class="choiceIndustryOne_header">
			<text>选择一个作为主营业务</text>
		</view>
		
		<!-- 选择 -->
		<view class="choiceIndustryOne_list">
			<div v-for="(item, index) in data" :key="index">
				<view class="choiceIndustryOne_list_item">
					<text>{{item}}</text>
				</view>
			</div>
		</view>
		
		<!-- 重新选择身份 下一步 -->
		<view class="choiceIndustry_pre_nex">
			<view class="choiceIndustry_pre_nex_left">
				<image class="choiceIndustry_pre_nex_left_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/left_right.png" mode=""></image>
				<text class="choiceIndustry_pre_nex_left_txt">重新选择身份</text>
			</view>
			<view class="choiceIndustry_pre_nex_right">
				<image class="choiceIndustry_pre_nex_right_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/right_icon.png" mode=""></image>
				<text class="choiceIndustry_pre_nex_right_txt">名称查询</text>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				data : [
					'服饰','箱包','鞋帽','母婴','美妆'
				]
			};
		}
	}
</script>

<style>
	
.choiceIndustryOne_header {
	margin-top: 150px;
	margin-bottom: 60px;
}
.choiceIndustryOne {
	text-align: center;
}
.choiceIndustryOne_list {
	
}

.choiceIndustryOne_list_item {
	margin: 0 auto;
	border: 1px solid #E77D53;
	width: 200upx;
	height: 34px;
	line-height: 34px;
	margin-top: 10px;
	border-radius: 5px;
	color: #E77D53;
	font-size: 14px;
}

.choiceIndustry_pre_nex {
	margin-top: 120px;
	margin-bottom: 55px;
	display: flex;
	flex-direction: row;
	justify-content: space-around;
}
.choiceIndustry_pre_nex_left {
	display: flex;
	flex-direction: column;
}

.choiceIndustry_pre_nex_right {
	display: flex;
	flex-direction: column;
	
	/* 灰色 */
	/* color: #dbdbdb; */
}

.choiceIndustry_pre_nex_left_img {
	margin: 0 auto;
	width: 40upx;
	height: 20px;
	margin-bottom: 10px;
}

 .choiceIndustry_pre_nex_right_img {
	margin: 0 auto;
	width: 40upx;
	height: 20px;
	margin-bottom: 10px;
 }
</style>
