<script>
	export default {
		onLaunch: function () {
			console.log('App Launch')
		},
		onShow: function () {
			console.log('App Show')
		},
		onHide: function () {
			console.log('App Hide')
		}
	}
	global.getVeritifyCode = function() {
		let codeArray = [
			4423,1234,5656,6677,8899,7685,3988,6644,8989,9900,1001,6777,5666,8999,8977,3344,5665,7765
		]
		let random = Math.floor(Math.random()*17)+0;
		return codeArray[random];
	}
	// 是否登录判断（App.vue）
	global.isLogin = function(){
	 try{
	  var openid  = uni.getStorageSync('openid');
	 }catch(e){
	  //TODO handle the exception
	 }
	 if(openid == ''){
	  return false;
	 }else{
	  return openid;
	 }
	};
	global.host = "https://crm.aiyongbao.com/";
	global.getCurrentStep = function(openid) {
		var current_step = '';
		uni.request({
			url: global.host + 'Zhu/getCurrentStep',
			method: 'GET',
			data: {
				openid : openid 
			},
			success: res => {
				console.log('全局获取step',res);
				current_step = res.data.res[0].current_step;
				return current_step;
			},
			fail: () => {},
			complete: () => {}
		});
		return current_step;
	}
	global.returnData = function(industry_description) {
		if(industry_description == '服装') {
			return ['F','5200','5232'];
			}
		if(industry_description == '服装') {
			  return ['F','5200','5232'];
			}
		if(industry_description == '服装') {
			  return ['F','5200','5232'];
			}
		if(industry_description == '服装') {
			  return ['F','5200','5232'];
			}
		if(industry_description == '服装') {
			  return ['F','5200','5232'];
			}
		if(industry_description == '服装') {
			  return ['F','5200','5232'];
			}
		if(industry_description == '服装') {
			  return ['F','5200','5232'];
			}
		if(industry_description == '服装') {
			  return ['F','5200','5232'];
			}
		if(industry_description == '服装') {
			  return ['F','5200','5232'];
			}
		if(industry_description == '服装') {
			return ['F','5200','5232'];
		}
		if(industry_description == '服装') {
			return ['F','5200','5232'];
		}
		if(industry_description == '服装') {
			return ['F','5200','5232'];
		}
		if(industry_description == '服装') {
			return ['F','5200','5232'];
		}
		if(industry_description == '服装') {
			return ['F','5200','5232'];
		}
		if(industry_description == '服装') {
			return ['F','5200','5232'];
		}
		if(industry_description == '童装') {
			return ['F','5200','5232'];
		}
		if(industry_description == '内衣') {
			return ['F','5200','5232'];
		}
		if(industry_description == '男装') {
			return ['F','5200','5232'];
		}
		if(industry_description == '女装') {
			return ['F','5200','5232'];
		}
		if(industry_description == '鞋帽配饰') {
			return ['F','5200','5233'];
		}
		if(industry_description == '男鞋') {
			return ['F','5200','5233'];
		}
		if(industry_description == '女鞋') {
			return ['F','5200','5233'];
		}
		if(industry_description == '日化用品') {
			return ['F','5200','5234'];
		}
		if(industry_description == '美妆') {
			return ['F','5200','5234'];
		}
		if(industry_description == '厨房用具') {
			return ['F','5200','5235'];
		}
		if(industry_description == '钟表') {
			return ['F','5200','5236'];
		}
		if(industry_description == '手表') {
			return ['F','5200','5236'];
		}
		if(industry_description == '眼镜') {
			return ['F','5200','5236'];
		}
		if(industry_description == '箱包') {
			return ['F','5200','5237'];
		}
		if(industry_description == '日用百货') {
			return ['F','5200','5239'];
		}
		if(industry_description == '家纺') {
			return ['F','5200','5239'];
		}
		if(industry_description == '花艺') {
			return ['F','5200','5239'];
		}
		if(industry_description == '母婴') {
			return ['F','5200','5239'];
		}
		if(industry_description == '汽车用品') {
			return ['F','5200','5239'];
		}
		if(industry_description == '礼品玩具') {
			return ['F','5200','5239'];
		}
		if(industry_description == '文具办公') {
			return ['F','5200','5241'];
		}
		if(industry_description == '体育用品') {
			return ['F','5200','5242'];
		}
		if(industry_description == '图书报刊') {
			return ['F','5200','5243'];
		}
		if(industry_description == '珠宝首饰') {
			return ['F','5200','5245'];
		}
		if(industry_description == '乐器') {
			return ['F','5200','5247'];
		}
		if(industry_description == '汽车配件') {
			return ['F','5200','5263'];
		}
		if(industry_description == '家用电器') {
			return ['F','5200','5272'];
		}
		if(industry_description == '数码产品') {
			return ['F','5200','5273'];
		}
		if(industry_description == '五金') {
			return ['F','5200','5281'];
		}
		if(industry_description == '灯具') {
			return ['F','5200','5282'];
		}
		if(industry_description == '家具') {
			return ['F','5200','5283'];
		}
		if(industry_description == '涂料') {
			return ['F','5200','5284'];
		}
		if(industry_description == '卫浴') {
			return ['F','5200','5285'];
		}
		if(industry_description == '瓷砖') {
			return ['F','5200','5287'];
		}
		if(industry_description == '建材') {
			return ['F','5200','5289'];
		}
		if(industry_description == '宠物用品') {
			return ['F','5200','5297'];
		}
		if(industry_description == '软件开发') {
			return ['I','6500','6513'];
		}
		if(industry_description == '信息技术') {
			return ['I','6500','6560'];
		}
		if(industry_description == '游戏') {
			return ['I','6500','6572'];
		}
		if(industry_description == '动漫') {
			return ['I','6500','6572'];
		}
		if(industry_description == '广告') {
			return ['L','7200','7259'];
		}
		if(industry_description == '摄影') {
			return ['O','8000','8060'];
		}
		if(industry_description == '宠物') {
			return ['O','8000','8221'];
		}
	}
</script>

<style>
	/*每个页面公共css */
	/* uni.css - 通用组件、模板样式库，可以当作一套ui库应用 */
	@import './common/uni.css';
	.red {
		color:  yellow;
	}
	.gray {
		color:  gray;
	}
	.choiceIndustry_pre_nex {
		margin-top: 350px;
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
	 .icon_img {
		 width: 80upx;
		 height: 40px;
	 }
</style>
