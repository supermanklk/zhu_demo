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
			<div v-for="(item, index) in popularData" data-id="popularData" :key="index" :data-popularDataIndex="index"  @click="clickItem">
				<view :class="item.active == true ? 'popularData_item active' : 'popularData_item' " >
					<text>{{item.name}}</text>
				</view>
			</div>
		</view>
		
		
		<!-- 更多 -->
		<view class="choiceIndustry_header_3">
			<text>更多</text>
		</view>
		<!-- 更多类目 -->
		<view class="popularData">
			<div v-for="(item, index) in moreData" data-id="moreData" :key="index" :data-moreDataIndex="index"  @click="clickItem">
				<view :class="item.active == true ? 'popularData_item active' : 'popularData_item' " >
					<text>{{item.name}}</text>
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
	// var count = 0;
	export default {
		data() {
			return {
				popularData : [
					{"name" : "服饰", "active" : false},
					{"name" : "箱包", "active" : false},
					{"name" : "鞋帽", "active" : false},
					{"name" : "母婴", "active" : false},
					{"name" : "美妆", "active" : false},
					{"name" : "数码", "active" : false},
					{"name" : "家纺", "active" : false},
					{"name" : "家电", "active" : false},
					{"name" : "家具", "active" : false},
					{"name" : "日用品", "active" : false},
					{"name" : "百货", "active" : false},
					{"name" : "农产品", "active" : false},
					{"name" : "办公用品", "active" : false},
					{"name" : "汽车用品", "active" : false},
					{"name" : "宠物用品", "active" : false},
					{"name" : "花艺", "active" : false},
					{"name" : "运动", "active" : false},
					{"name" : "软件", "active" : false},
					{"name" : "摄影", "active" : false},
					{"name" : "广告", "active" : false}
				],
				moreData : [
					{"name" : "男装", "active" : false},
					{"name" : "女装", "active" : false},
					{"name" : "童装", "active" : false},
					{"name" : "内衣", "active" : false},
					{"name" : "男鞋", "active" : false},
					{"name" : "女鞋", "active" : false},
					{"name" : "化妆品", "active" : false},
					{"name" : "床品", "active" : false},
					{"name" : "眼镜", "active" : false},
					{"name" : "钟表", "active" : false},
					{"name" : "手表", "active" : false},
					{"name" : "乐器", "active" : false},
					{"name" : "建材", "active" : false},
					{"name" : "卫浴", "active" : false},
					{"name" : "灯具", "active" : false},
					{"name" : "工具", "active" : false},
					{"name" : "文具", "active" : false},
					{"name" : "玩具", "active" : false},
					{"name" : "书刊", "active" : false},
					{"name" : "琴行", "active" : false},
					{"name" : "手机", "active" : false},
					{"name" : "通讯", "active" : false},
					{"name" : "鲜花", "active" : false},
					{"name" : "珠宝", "active" : false},
					{"name" : "汽车", "active" : false},
					{"name" : "户外", "active" : false},
					{"name" : "信息技术", "active" : false},
					{"name" : "信息咨询", "active" : false},
					{"name" : "宠物", "active" : false},
					{"name" : "游戏", "active" : false},
					{"name" : "动漫", "active" : false},
					{"name" : "影视", "active" : false}
				],
				popularActive : [
					
				],
				moreActive : [
					
				]
			};
		},
		methods: {
			clickItem(e) {
				console.log(e);
				let count = 0;
				// 点击之前要计算是否超过5个激活了,如果超过5个就不能够再操作
				for(let i = 0; i < this.moreData.length; i++) {
					if(this.moreData[i].active == true) {
						count++;
					}
				}
				for(let i = 0; i < this.popularData.length; i++) {
					if(this.popularData[i].active == true) {
						count++;
					}
				}
				let status = false; //查看当前状态,如果是取消则没有限制
				if(e.currentTarget.dataset.moredataindex) { // 更多
					status = this.moreData[e.currentTarget.dataset.moredataindex].active;
				} else { // 热门
					status = this.popularData[e.currentTarget.dataset.populardataindex].active;
				}
				// 这里判断为4是首次计算的之前的,未包含本次的,加上本次的就是5了.   status 是如果用户是取消选项,则没有限制
				if(count <= 4 || status) {
					if(e.currentTarget.dataset.moredataindex || e.currentTarget.dataset.moredataindex == 0) {
						console.log(e.currentTarget.dataset.moredataindex);
						let index = e.currentTarget.dataset.moredataindex;
						this.moreData[index]['active'] = !this.moreData[index]['active'];
					} else {
						console.log(e);
						console.log(e.currentTarget.dataset.populardataindex);
						let index = e.currentTarget.dataset.populardataindex;
						this.popularData[index]['active'] = !this.popularData[index]['active'];
					}
				} else {
					uni.showToast({
						title: '最多选5项哦!',
						duration: 2000,
						icon :'none'
					});
				}
				
			}
		}
	}
</script>

<style>
.active {
	background: #E77D53;
	color: #FFFFFF!important;
}
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
	height: 80px;
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
