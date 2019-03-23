<!-- 选择行业 -->
<template>
	<view class="choiceIndustry">
		<view class="choiceIndustry_header">
			
			<view class="choiceIndustry_header_3">
				<text>选择你的行业</text>
			</view>
			<view class="choiceIndustry_header_1">
				<text>{{activeArr == '' ? title : '(已选'+activeArr.length+'/5:  '+activeArr+')'}}</text>
			</view>
			<!-- <view class="choiceIndustry_header_2">
				<text>{{activeArr == '' ? '(最多选5项)' : '综合税率5%'}}</text>
			</view> -->
			<!-- <view class="choiceIndustry_header_3">
				<text>热门</text>
			</view> -->
		</view>
		
		<!-- 热门类目 -->
		<view class="popularData">
			<div v-for="(item, index) in popularData" data-id="popularData" :key="index">
				<view :key="index" :data-popularDataIndex="index"  @click="clickItem" :class="item.active == true ? 'popularData_item active' : 'popularData_item' " >
					<text>{{item.name}}</text>
				</view>
			</div>
		</view>
		
		
		<!-- 更多 -->
		<!-- <view class="choiceIndustry_header_3">
			<text>更多</text>
		</view> -->
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
			<view class="choiceIndustry_pre_nex_right" @click="clickNext">
				<image class="choiceIndustry_pre_nex_right_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/right_icon.png" mode=""></image>
				<!-- <image class="choiceIndustry_pre_nex_right_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/right_icon_gray.png" mode=""></image> -->
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
				title : '(最多选5项)',
				activeArr : '',
				popularData : [
					{"name" : "服装", "active" : false},
					{"name" : "箱包", "active" : false},
					{"name" : "鞋帽", "active" : false},
					{"name" : "美妆个护", "active" : false},
					{"name" : "母婴百货", "active" : false},
// 					{"name" : "家纺", "active" : false},
// 					{"name" : "日化用品", "active" : false},
// 					{"name" : "体育用品", "active" : false},
// 					{"name" : "厨房用具", "active" : false},
// 					{"name" : "家用电器", "active" : false},
// 					{"name" : "文具办公", "active" : false},
// 					{"name" : "数码产品", "active" : false},
// 					{"name" : "珠宝首饰", "active" : false},
// 					{"name" : "日用百货", "active" : false},
// 					{"name" : "礼品玩具", "active" : false},
// 					{"name" : "宠物用品", "active" : false},
				],
				moreData : [
// 					{"name" : "男装", "active" : false},
// 					{"name" : "女装", "active" : false},
// 					{"name" : "童装", "active" : false},
// 					{"name" : "内衣", "active" : false},
// 					{"name" : "男鞋", "active" : false},
// 					{"name" : "女鞋", "active" : false},
// 					{"name" : "钟表", "active" : false},
// 					{"name" : "手表", "active" : false},
// 					{"name" : "眼镜", "active" : false},
// 					{"name" : "家具", "active" : false},
// 					{"name" : "灯具", "active" : false},
// 					{"name" : "涂料", "active" : false},
// 					{"name" : "五金", "active" : false},
// 					{"name" : "卫浴", "active" : false},
// 					{"name" : "瓷砖", "active" : false},
// 					{"name" : "建材", "active" : false},
// 					{"name" : "图书报刊", "active" : false},
// 					{"name" : "乐器", "active" : false},
// 					{"name" : "汽车用品", "active" : false},
// 					{"name" : "汽车配件", "active" : false},
// 					{"name" : "游戏", "active" : false},
// 					{"name" : "动漫", "active" : false},
// 					{"name" : "宠物", "active" : false},
// 					{"name" : "花艺", "active" : false},
// 					{"name" : "摄影", "active" : false},
// 					{"name" : "软件开发", "active" : false},
// 					{"name" : "信息技术", "active" : false},
// 					{"name" : "广告", "active" : false},
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
				let activeArr = [];
				for(let i = 0; i < this.popularData.length; i++) {
					if(this.popularData[i].active == true) {
						activeArr.push(this.popularData[i].name);
					}
				}
				for(let n = 0; n < this.moreData.length; n++) {
					if(this.moreData[n].active == true) {
						activeArr.push(this.moreData[n].name);
					}
				}
				// 已选的类目
				this.activeArr = activeArr;
			},
			returnPre() {
				uni.navigateBack({
					delta: 1
				});
			},
			clickNext() {
				// 点击下一步必须选择至少一个经营类目
				if(this.activeArr.length >=1) {
					// 将选择的范围存到缓存中
					try {
						uni.setStorageSync('business_scope', this.activeArr);
					} catch (e) {
						// error
					}
					// 修改step
					try {
						const openid = uni.getStorageSync('openid');
						if (openid) {
								uni.request({
								url: global.host + 'Zhu/editCurrentStep',
								method: 'GET',
								data: {
									openid : openid,
									current_step : 1 // 0代表处理选择行业阶段
								},
								success: res => {
									uni.reLaunch({
										url: '../main_index/main_index?from=choiceIndustryOne'
									});
								},
								fail: () => {},
								complete: () => {}
							});
						}
					} catch (e) {
						// error
						console.log('error888',e);
					}
				} else {
					uni.showToast({
						title: '请先选择类目',
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
	margin-top: 20px;
	margin-bottom: 70px;
}
.choiceIndustry_header_2 {
	margin-top: 5px;
}
.choiceIndustry_header_3 {
	font-size: 14px;
	margin-top: 15px;
}
.popularData {
	width: 660upx;
	margin: 0 auto;
	display: flex;
	flex-direction: column;
	align-content: center;
	/* justify-content: space-between; */
}
.popularData_item {
	margin: 0 auto;
	font-size: 14px;
	color: #E77D53;
	border: 1px solid #E77D53;
	width: 200upx;
	height: 34px;
	line-height: 34px;
	margin-top: 10px;
	border-radius: 5px;
}
.choiceIndustry_pre_nex {
	margin-top: 140px;
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
