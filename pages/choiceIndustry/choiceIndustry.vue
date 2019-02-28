<!-- 选择行业 -->
<template>
	<view class="choiceIndustry">
		<view class="choiceIndustry_header">
			<view class="choiceIndustry_header_1">
				<text>选择你的行业</text>
			</view>
			<view class="choiceIndustry_header_2">
				<text>(最多选5项)</text>
			</view>
			<view class="choiceIndustry_header_3">
				<text>热门</text>
			</view>
		</view>
		
		<!-- 热门类目 -->
		<view class="popularData">
			<div v-for="(item, index) in popularData" :key="index" :data-popularDataIndex="index"  @click="clickItem">
				<view class="popularData_item">
					<text>{{item}}</text>
				</view>
			</div>
		</view>
		
		
		<!-- 更多 -->
		<view class="choiceIndustry_header_3">
			<text>更多</text>
		</view>
		<!-- 更多类目 -->
		<view class="popularData">
			<div v-for="(item, index) in moreData" :key="index" :data-popularDataIndex="index"  @click="clickItem">
				<view class="popularData_item" >
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
				<image class="choiceIndustry_pre_nex_right_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/right_icon_gray.png" mode=""></image>
				<text class="choiceIndustry_pre_nex_right_txt">下一步</text>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				popularData : [
					'服饰','箱包','鞋帽','母婴','美妆','数码','家纺','家电','家具','日用品','百货','农产品','办公用品','汽车用品','宠物用品','花艺','运动','软件','摄影','广告'
				],
				moreData : ['男装','女装','童装','内衣','男鞋','女鞋','化妆品','床品','眼镜','钟表','手表','乐器','建材','卫浴','灯具','工具','文具','玩具','书刊','琴行','手机','通讯','鲜花','珠宝','汽车','户外','信息技术','信息咨询','宠物','游戏','动漫','影视'
				]
			};
		},
		methods: {
			clickItem(e) {
				console.log(e,333);
			}
		}
	}
</script>

<style>
.choiceIndustry {
	text-align: center;
	font-size: 15px;
	color: #000000;
	font-style: normal;
	font-family: "PingFang SC", "Hiragino Sans GB", "Heiti SC", "Microsoft YaHei", "WenQuanYi Micro Hei";
}

.choiceIndustry_header_1 {
	margin-top: 25px;
}
.choiceIndustry_header_2 {
	margin-top: 5px;
}
.choiceIndustry_header_3 {
	margin-top: 15px;
}
.popularData {
	width: 660upx;
	margin: 0 auto;
	display: flex;
	flex-wrap: wrap;
	justify-content: space-between;
}
.popularData_item {
	font-size: 14px;
	color: #E77D53;
	border: 1px solid #E77D53;
	width: 150upx;
	height: 25px;
	margin-top: 10px;
	border-radius: 5px;
}
.choiceIndustry_pre_nex {
	margin-top: 50px;
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
	color: #dbdbdb;
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
