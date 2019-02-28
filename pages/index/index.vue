<template>
	<view class="content">
		<!-- 步骤介绍 -->
		<view class="step">
			<text class="stepTitle">
				简单5步在线申请个体营业执照
			</text>
		</view>
		
		<!-- 宣传区域 -->
		<view class="promotionArea">
			
		</view>
		
		<!-- 立即申请 -->
		<!-- <view class="applyImmediately">
			<button type="" size="mini" plain="true" style="width: 360upx; color: #1AAD19; border-color: #1AAD19;" hover-class = "btn_hover">
				立即申请
			</button>
		</view> -->
		
		<!-- #ifdef MP-WEIXIN -->  
		<view class="applyImmediately">
			<button  size="mini" plain="true" style="width: 360upx; color: #1AAD19; border-color: #1AAD19;"  hover-class = "btn_hover" open-type="getPhoneNumber" bindgetphonenumber="getPhoneNumber">立即申请</button>
		</view>
		<!-- #endif -->  
		
		
		
		<!-- 常见问题 -->
		<view class="">
			<text class="commonProblem">
				常见问题
			</text>
		</view>
		
	</view>
</template>

<script>
	var _left;
	export default {
		data() {
			return {
				activeIndex : 0,
				news : [
				],
				music : "http://img-cdn-qiniu.dcloud.net.cn/uniapp/audio/music.mp3",
				zhangbin : [
					{ name : 'zhangbin', sex : '男'},
					{ name : '小晨', sex : 'girl'},
					{ name : '小丁', sex : 'girl'}
				],
				age : 20,
				text : true,
				isRed : true,
				menus : [
					'新闻',
					'图片',
					'音乐'
				]
			}
		},
		onLoad() {
			_left = this;
			
			
			
			// 当页面加载成功以后,会进行一次远程数据的请求
// 			uni.request({
// 				url: 'https://unidemo.dcloud.net.cn/api/news',
// 				method: 'GET',
// 				data: {},
// 				success: res => {
// 					console.log(res);
// 					this.news = res.data;
// 				},
// 				fail: () => {},
// 				complete: () => {}
// 			});
			
// 			setTimeout(function(){
// 				// 这里的this就不是外面的this了.
// 				// 这里是3秒以后改变age的值
// 				_left.age = 30;
// 			},3000)
		},
		onShow() {
			console.log('页面显示1');
		},
		onHide() {
			console.log('页面隐藏1');
		},
		methods: {
			openinfo(e) {
				var newsid = e.currentTarget.dataset.newsid;
				uni.navigateTo({
					url: '../info/info?newsid=' + newsid
				});
			},
			menuClick (e) {
				console.log(e.currentTarget.id);
				this.activeIndex = e.currentTarget.id;
			},
			img () {
				uni.chooseImage({
					count : 3,
					sizeType : "compressed",
					success : function(res) {
						console.log(res);
					}
				})
			}
		}
	}
</script>

<style>
	.uni-media-list-body{height: auto;}
	.uni-media-list-text-top{line-height: 1.6em;}
	.menus {
		float: left;
		margin-left: 10upx;
	}
	.activeStyle {
		color: red;
	}
	
	.content {
		font-style: normal;
		text-align: center;
	}
	
	.step {
		margin-top: 25px;
		margin-bottom: 25px;
		text-align: center;
	}
	.stepTitle {
		font-size: 17px;
	}
	.promotionArea {
		margin:  0 auto;
		width: 670upx;
		height: 429px;
		background: #D8D8D8;
	}
	
	/* 立即申请 */
	.applyImmediately {
		margin-top: 25px;
		margin-bottom: 25px;
	}
	.btn_hover {
		color: black!important;
	}
	
	/* 常见问题 */
	.commonProblem {
		color: #BD10E0;
		font-size: 10px;
	}
</style>
