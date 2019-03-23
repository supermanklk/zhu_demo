<template>
	<view>
		<view class="review">
			<image class="review_header_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/wait.png" mode=""></image>
			<text class="review_header_txt">提交成功,请耐心等待审核</text>
		</view>
		
		<view class="review_content">
				<view class="review_content_left">
					<view class="review_content_left_step1">
						<image class="current_progress" style="position: absolute;top: 0;" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/current_progress.png" mode=""></image>
					</view>
					<view class="review_content_left_center">
					</view>
					<view class="review_content_left_step2">
						<image class="gray_spot" style="position: absolute;top: 80px;left: 15upx;" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/Zhu/icon/gray_spot.png" mode=""></image>
					</view>
				</view>
				<view class="review_content_right">
					<view class="review_content_right_step1">
						<text>审核中</text> <text style="color: #999999;margin-left: 42upx; ">(待完成)</text>
					</view>
					<view class="review_content_right_step2">
						<text>领取公章及税盘</text><text style="color: #999999;margin-left: 20upx; ">(待完成)</text>
					</view>
				</view>
		</view>
		
		<view class="datainfo">
			<text>
				{{datainfo}}
			</text>
		</view>
		
		<!-- 返回主页 -->
		<view class="review_return_main">
			<button type="" @click="goMain_index" size="mini" plain="true" style="width: 360upx; color: #1AAD19; border-color: #1AAD19;" hover-class = "btn_hover">
				返回主页
			</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				datainfo : ''
			};
		},
		onLoad() {
			console.log('进度页面');
				try {
					const value = uni.getStorageSync('beian');
					if (value.data) {
						console.log(value);
						this.datainfo = value.data;
					} else {
						const openid = uni.getStorageSync('openid');
						// 如果本地缓存没有则去接口请求
						uni.request({
							url: global.host + 'Zhu/getCompanyInfo',
							method: 'GET',
							data: {
								openid : openid
							},
							success: res => {
								console.log('res11',res);
								let notification = JSON.parse(res.data.data["0"].notification).data;
								this.datainfo = notification;
							},
							fail: () => {},
							complete: () => {}
						});
					}
				} catch (e) {
					// error
					console.log('error,进度页面');
				}
		},
		methods:{
			goMain_index() {
				// 返回主页
				uni.reLaunch({
					url: '../main_index/main_index?from=review'
				});
			}
		}
	}
</script>

<style>
.datainfo {
	margin: 0 auto;
	margin-top: 20px;
	width: 600upx;
	font-size: 14px;
	border: 1px solid #4CD964;
	border-radius: 5px;
	padding: 5px;
}
.review {
	text-align: center;
	display: flex;
	flex-direction: column;
}
.review_header_img {
	margin: 0 auto;
	margin-top: 50px;
	width: 80upx;
	height: 40px;
}
.review_header_txt {
	margin-top: 34px;
	font-size: 14px;
}
.review_content {
	display: flex;
	flex-direction: row;
	width: 436upx;
	margin-left: 202upx;
	margin-top: 50px;
}
.review_content_right {
	display: flex;
	flex-direction: column;
	margin-left: 39upx;
}
.review_content_right_step1 {
	display: flex;
	flex-direction: row;
}
.review_content_right_step2{
	margin-top: 45px;
	display: flex;
	flex-direction: row;
}
.review_content_left {
	position: relative;
	display: flex;
	flex-direction: column;
	justify-content: center;
	margin-left: 20upx;
	margin-right: 35upx;
}
.review_content_left_center {
	width: 2px;
	height: 70px;
	background: #DDDDDD;
	margin-left: 28upx;
}
.gray_spot {
	width: 30upx;
	height: 15px;
}
.current_progress {
	width: 60upx;
	height: 30px;
}
.review_return_main {
	margin-top: 120px;
	text-align: center;
	padding-bottom: 50px;
}
</style>
