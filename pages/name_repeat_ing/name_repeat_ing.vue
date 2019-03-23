<template>
	<view>
		<view class="name_repeat_ing">
			<image class="an name_repeat_ing_img" src="http://qniyong.oss-cn-hangzhou.aliyuncs.com/item/web/images/circle.png" mode=""></image>
			<text style="font-size: 20px; color: #000000; margin-top: 16px;">查询中</text>
			<text style="color: #888888; font-size: 13px; border-bottom: 1px solid #E5E5E5; margin-top: 5px; padding-bottom: 20px; font-weight: 400;">大约还需要{{second}}秒,请不要关闭页面</text>
		</view>
	</view>
</template>

<script>
	var _self,timer,account,password,randomAccount = [],morerandomAccount = [],accounts = 'supermanzhangbin,18602197097,18361272556,18717715286,13918418490,17317277937'; // randomAccount 为可以选择的账号
	export default {
		data() {
			return {
				second : 30
			};
		},
		onUnload() {
			timer = null;
		},
		onLoad(e) {
			_self = this;
			account = 'supermanzhangbin'; // 默认登录账号
			password = 'faith1314z'; 
			console.log(e);
			// 在加载之前 需要登录的账号信息,因为查询库内各个账号使用的情况 统计出账号下没有超过超过10个 随机放入
			uni.request({
				url: global.host + 'Zhu/getInfoAccount',
				method: 'GET',
				data: {
					accounts : accounts
				},
				success: res => {
					morerandomAccount = [];
					console.log('查看字号库内各个账号使用的情况',res);
					// 统计账号下没有超过10个的
					let data = res.data;
					for(let i = 0; i < data.length; i ++ ) {
						if(data[i].data.length < 10) {
							randomAccount.push(data[i].account);
						} else {
							// 超过的
							morerandomAccount.push(data[i].account);
						}
					}
// 					// 如果账号既没有在randomAccount 也没有在morerandomAccount 说明 里面为空,则就属于可以添加字号的账号
// 					let accountArr = account.split(',');
// 					for(let i = 0; i < accountArr.length; i ++) {
// 						if(randomAccount.indexOf(accountArr[i]) && morerandomAccount.indexOf(accountArr[i]) )
// 						// 说明账户下没有字号,可以添加
// 						randomAccount.push(accountArr[i]);
// 					}
// 					
					console.log('randomAccount',randomAccount);
					// 登录的账号是
					// return;
					account = randomAccount[global.randomNum(0,randomAccount.length-1)];
					password = global.getPassword(account);
					console.log('登录的账号是',account,password);
					// console.log(global.randomNum(0,randomAccount.length-1));
					
					const {business_big, business_center, business_small, character, organization, business} = e;
					timer = setInterval(() => {
						_self.second = _self.second - 1;
					},1200);
					console.log(business_big, business_center, business_small, character, organization, business);
					uni.request({
						url: global.host + 'Zhu/getName?character='+character + '&business=' + business,
						// url: 'http://crm.binbin.aiyongbao.com/Zhu/getName?character='+character + '&business=' + business,
						method: 'GET',
						data: {
							business_big : business_big,
							business_center : business_center,
							business_small : business_small,
							organization : organization,
							account : account,
							password : password,
						},
						success: res => {
							clearInterval(timer);
							console.log('325435',res);
							// uni.hideLoading();
							if(res.data) {
								timer = null;
								console.log('查重报告',res);
								if(res.data.indexOf('查重通过') != -1) {
									// 需要向数据库存个它的名字
									uni.request({
										url: global.host + 'Zhu/setCharacter',
										method: 'GET',
										data: {
											openid : global.openid,
											character : decodeURIComponent(character),
											account : account,
											password : password,
										},
										success: res => {
											console.log('成功时候插入字号库的返回结果',res);
										},
										fail: () => {},
										complete: () => {}
									});
									
									
									// 查询通过,把这个字号給它缓存到本地
									try {
										uni.setStorageSync('bussiness_name', res.data.split('，')[0]);
										uni.redirectTo({
											url: '../nameRepeatSuccess/nameRepeatSuccess?name=' + character + '&business=' + business + '&organization=' + organization
										});	
									} catch (e) {
										// error
									}
									
								} else {
									// 说明查重失败
									console.log('查重失败',res.data);
									if( res.data== '400312') {
										// 登录失败 重新加载
										uni.showToast({
											title: '系统异常,重新查重',
											duration: 2000,
											icon : 'none'
										});
										uni.redirectTo({
											url: '../nameRepeatError/nameRepeatError?errormsg=' + res.data
										});	
									} else {
										uni.redirectTo({
											url: '../nameRepeatError/nameRepeatError?errormsg=' + res.data
										});	
									}
								}
								
							}
						},
						fail: (e) => {
							clearInterval(timer);
							console.log('查重报错',e);
							uni.hideLoading();
							uni.showToast({
								title: '系统繁忙,请重试',
								duration: 2000
							});
						},
						complete: () => {}
					});
					
					
				},
				fail: () => {},
				complete: () => {}
			});
			
		}
	}
</script>

<style>
.name_repeat_ing {
	margin-top: 50px;
	display: flex;
	flex-direction: column;
	text-align: center;
	
}
.name_repeat_ing_img {
	margin: 0 auto;
	width: 120upx;
	height: 60px;
}

.an{

    -webkit-transform: rotate(360deg);

    animation: rotation 2 linear infinite;

    -moz-animation: rotation 2 linear infinite;

    -webkit-animation: rotation 2s linear infinite;

    -o-animation: rotation 2s linear infinite;

}
@-webkit-keyframes rotation{

    from {-webkit-transform: rotate(0deg);}

    to {-webkit-transform: rotate(360deg);}

}


</style>
