<template>
	<view class="content">
		<view class="uni-list">
			<view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(item,index) in news" :key="index" @tap="openinfo" :data-newsid="item.post_id">
				<view class="uni-media-list">
					<image class="uni-media-list-logo" :src="item.author_avatar"></image>
					<view class="uni-media-list-body">
						<view class="uni-media-list-text-top">{{item.title}}{{age}}</view>
						<view class="uni-media-list-text-bottom uni-ellipsis">{{item.created_at}}</view>
					</view>
				</view>
			</view>
		</view>
		<!-- 列表循环渲染,基础语法就是 v-for -->
		<view class="" v-for="(item,index2) in zhangbin" :key="index2">
			{{item.name}} -- {{item.sex}}
		</view>
		<!-- 条件渲染 -->
		<view  v-if="text">
			text.......
		</view>
		<!-- 三元操作符 -->
		<view v-bind:style="{ color: isRed ? 'red' : ''}">666555</view>
		<!-- class 判断 -->
		<view :class="[isRed ? 'red' : 'gray']">111</view>
		<!-- 平常加 : 表示是动态的 -->
		
		<view class="menus" v-for="(item,index3) in menus" :key="index3" :class="[activeIndex == index3 ? 'activeStyle' : '']" @click="menuClick" :id=index3>
			{{item}}
		</view>
		<!-- 音频 -->
		<view class="">
			<audio  :src="music" controls></audio>
		</view>
		<!-- 照相 -->
		<view class="">
			<button type="primary" @click="img">拍照</button>
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
			uni.request({
				url: 'https://unidemo.dcloud.net.cn/api/news',
				method: 'GET',
				data: {},
				success: res => {
					console.log(res);
					this.news = res.data;
				},
				fail: () => {},
				complete: () => {}
			});
			
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
</style>
