<script>
	export default {
		onLaunch: function () {
			console.log('App Launch')
		},
		onShow: function () {
			console.log('App Show')
			try {
				const openid = uni.getStorageSync('openid');
				if (openid) {
					console.log('设置全局global');
					global.openid = openid;
					console.log(global.openid);
				}
			} catch (e) {
				// error
			}
		},
		onHide: function () {
			console.log('App Hide')
		}
	}
	global.business_scope_return = function(business_scope_text) {
		business_scope_text = business_scope_text.replace(/服装/,'服装，');
		business_scope_text= business_scope_text.replace(/鞋帽/,'鞋帽配饰，');
		business_scope_text=business_scope_text.replace(/母婴百货/,'母婴用品及日用百货，');
		business_scope_text =business_scope_text.replace(/箱包/,'箱包，');
		business_scope_text = business_scope_text.replace(/美妆个护/,'化妆品及卫生用品，');
		business_scope_text = business_scope_text.replace(/健康销售/,'预包装食品，');
		business_scope_text = business_scope_text.replace(/\,/g,'');
		let str = '网上销售：' + business_scope_text;
		return str.slice(0,-1);
	}
	global.randomNum = function(minNum,maxNum){ 
		switch(arguments.length){ 
			case 1: 
				return parseInt(Math.random()*minNum+1,10); 
			break; 
			case 2: 
				return parseInt(Math.random()*(maxNum-minNum+1)+minNum,10); 
			break; 
				default: 
					return 0; 
				break; 
		} 
	} 
	
	global.getPassword = function(account) {
		switch(account) {
			case 'supermanzhangbin' :
				return 'faith1314z';
			break;
			case '18602197097' : 
				return 'loveapp103';
			break;
			case '18361272556':
				return 'tian18361272556';
			break;
			case '18717715286' : 
				return 'sunk333.';
			break;
			case '13918418490' :
				return 'zhoumeng1234';
			break;
			case '17317277937' :
				return 'zhoumeng1234';
			break;
		}
	}
	
	
	global.verificationContent = function(text) {
		let returntxt = '';
		let num = 0;
		if(text.length > 4) {
			returntxt = '字号需要为2~4个简体汉字';
			return returntxt;
		}
		if(!/^[\u4e00-\u9fa5]+$/gi.test(text)) {
			returntxt = '字号不能含英文、标点符号、阿拉伯数字';
			return returntxt;
		}
		for(let i = 0; i < text.length; i ++ ) {
			if(text[i].indexOf('一') != -1|| text[i].indexOf('二') != -1|| text[i].indexOf('三') != -1|| text[i].indexOf('四') != -1|| text[i].indexOf('五') != -1|| text[i].indexOf('六') != -1|| text[i].indexOf('七') != -1|| text[i].indexOf('八') != -1|| text[i].indexOf('九') != -1|| text[i].indexOf('十') != -1|| text[i].indexOf('壹') != -1|| text[i].indexOf('贰')!= -1|| text[i].indexOf('叁') != -1|| text[i].indexOf('肆') != -1|| text[i].indexOf('伍') != -1|| text[i].indexOf('陆')!= -1 || text[i].indexOf('柒') != -1|| text[i].indexOf('捌')!= -1|| text[i].indexOf('玖') != -1|| text[i].indexOf('拾')!= -1) {
				num ++;
			}
		}
		console.log(num);
		if(num >= 2) {
			return '不能含两个(含)以上大写数字"一、二、三、四、五、六、七、八、九、十","壹、贰、叁、肆、伍、陆、柒、捌、玖、拾"';
		}
		return 'ok';
	}
	global.clearAll = function() {
		uni.showModal({
			title: '提示',
			content: '重新申请将会清空完成的所有步骤',
			success: function (res) {
				if (res.confirm) {
					console.log('用户点击确定');
					// 清空数据库
					uni.request({
						url: global.host + 'Zhu/initialization',
						method: 'GET',
						data: {
							openid : global.openid,
							type : 3 // 在2个表的记录都删除
						},
						success: res => {
							console.log('数据库清理成功');
							// 清除本地缓存
							try {
								uni.clearStorageSync();
								console.log('本地缓存清理成功');
								uni.redirectTo({
									url: '../index/index'
								});
							} catch (e) {
								// error
							}
						},
						fail: () => {},
						complete: () => {}
					});
					
				} else if (res.cancel) {
					console.log('用户点击取消');
				}
			}
		});
	}
	global.getVeritifyCode = function() {
		let codeArray = [
			4423,1234,5656,6677,8899,7685,3988,6644,8989,9900,1001,6777,5666,8999,8977,3344,5665,7765
		]
		let random = Math.floor(Math.random()*17)+0;
		return codeArray[random];
	}
	// 判断身份证的基本信息
	global.verificationidcard = function(address,born,idcard,nation,orderid,realname,sex,side) {
		// born 需要8位
		// idcard 需要18位
		// sex必须是1位
		if(address == '' || born == '' || idcard == '' || nation == '' || realname == '' || sex == '') {
			return 'errorinfo'; // 信息缺少,请重新上传
		}
		console.log('born.length',born.length);
		if(born.length != 8) {
			return 8;
		}
		if(idcard.length != 18) {
			return 18;
		}
		if(sex.length != 1) {
			return 'error_sex';
		}
		return 'ok';
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
	if(industry_description == '鞋帽') {
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
	if(industry_description == '美妆个护') {
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
	if(industry_description == '母婴百货') {
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
	if(industry_description == '健康销售') {
		return ['F','5200','5226'];
	}
	}
	
	
	// md5加密区域
	
global.md5 =function(string) {
  var x = Array();
  var k, AA, BB, CC, DD, a, b, c, d;
  var S11 = 7, S12 = 12, S13 = 17, S14 = 22;
  var S21 = 5, S22 = 9, S23 = 14, S24 = 20;
  var S31 = 4, S32 = 11, S33 = 16, S34 = 23;
  var S41 = 6, S42 = 10, S43 = 15, S44 = 21;
  string = global.Utf8Encode(string);
  x = global.ConvertToWordArray(string);
  a = 0x67452301;
  b = 0xEFCDAB89;
  c = 0x98BADCFE;
  d = 0x10325476;
  for (k = 0; k < x.length; k += 16) {
    AA = a;
    BB = b;
    CC = c;
    DD = d;
    a = global.FF(a, b, c, d, x[k + 0], S11, 0xD76AA478);
    d = global.FF(d, a, b, c, x[k + 1], S12, 0xE8C7B756);
    c = global.FF(c, d, a, b, x[k + 2], S13, 0x242070DB);
    b = global.FF(b, c, d, a, x[k + 3], S14, 0xC1BDCEEE);
    a = global.FF(a, b, c, d, x[k + 4], S11, 0xF57C0FAF);
    d = global.FF(d, a, b, c, x[k + 5], S12, 0x4787C62A);
    c = global.FF(c, d, a, b, x[k + 6], S13, 0xA8304613);
    b = global.FF(b, c, d, a, x[k + 7], S14, 0xFD469501);
    a = global.FF(a, b, c, d, x[k + 8], S11, 0x698098D8);
    d = global.FF(d, a, b, c, x[k + 9], S12, 0x8B44F7AF);
    c = global.FF(c, d, a, b, x[k + 10], S13, 0xFFFF5BB1);
    b = global.FF(b, c, d, a, x[k + 11], S14, 0x895CD7BE);
    a = global.FF(a, b, c, d, x[k + 12], S11, 0x6B901122);
    d = global.FF(d, a, b, c, x[k + 13], S12, 0xFD987193);
    c = global.FF(c, d, a, b, x[k + 14], S13, 0xA679438E);
    b = global.FF(b, c, d, a, x[k + 15], S14, 0x49B40821);
    a = global.GG(a, b, c, d, x[k + 1], S21, 0xF61E2562);
    d = global.GG(d, a, b, c, x[k + 6], S22, 0xC040B340);
    c = global.GG(c, d, a, b, x[k + 11], S23, 0x265E5A51);
    b = global.GG(b, c, d, a, x[k + 0], S24, 0xE9B6C7AA);
    a = global.GG(a, b, c, d, x[k + 5], S21, 0xD62F105D);
    d = global.GG(d, a, b, c, x[k + 10], S22, 0x2441453);
    c = global.GG(c, d, a, b, x[k + 15], S23, 0xD8A1E681);
    b = global.GG(b, c, d, a, x[k + 4], S24, 0xE7D3FBC8);
    a = global.GG(a, b, c, d, x[k + 9], S21, 0x21E1CDE6);
    d = global.GG(d, a, b, c, x[k + 14], S22, 0xC33707D6);
    c = global.GG(c, d, a, b, x[k + 3], S23, 0xF4D50D87);
    b = global.GG(b, c, d, a, x[k + 8], S24, 0x455A14ED);
    a = global.GG(a, b, c, d, x[k + 13], S21, 0xA9E3E905);
    d = global.GG(d, a, b, c, x[k + 2], S22, 0xFCEFA3F8);
    c = global.GG(c, d, a, b, x[k + 7], S23, 0x676F02D9);
    b = global.GG(b, c, d, a, x[k + 12], S24, 0x8D2A4C8A);
    a = global.HH(a, b, c, d, x[k + 5], S31, 0xFFFA3942);
    d = global.HH(d, a, b, c, x[k + 8], S32, 0x8771F681);
    c = global.HH(c, d, a, b, x[k + 11], S33, 0x6D9D6122);
    b = global.HH(b, c, d, a, x[k + 14], S34, 0xFDE5380C);
    a = global.HH(a, b, c, d, x[k + 1], S31, 0xA4BEEA44);
    d = global.HH(d, a, b, c, x[k + 4], S32, 0x4BDECFA9);
    c = global.HH(c, d, a, b, x[k + 7], S33, 0xF6BB4B60);
    b = global.HH(b, c, d, a, x[k + 10], S34, 0xBEBFBC70);
    a = global.HH(a, b, c, d, x[k + 13], S31, 0x289B7EC6);
    d = global.HH(d, a, b, c, x[k + 0], S32, 0xEAA127FA);
    c = global.HH(c, d, a, b, x[k + 3], S33, 0xD4EF3085);
    b = global.HH(b, c, d, a, x[k + 6], S34, 0x4881D05);
    a = global.HH(a, b, c, d, x[k + 9], S31, 0xD9D4D039);
    d = global.HH(d, a, b, c, x[k + 12], S32, 0xE6DB99E5);
    c = global.HH(c, d, a, b, x[k + 15], S33, 0x1FA27CF8);
    b = global.HH(b, c, d, a, x[k + 2], S34, 0xC4AC5665);
    a = global.II(a, b, c, d, x[k + 0], S41, 0xF4292244);
    d = global.II(d, a, b, c, x[k + 7], S42, 0x432AFF97);
    c = global.II(c, d, a, b, x[k + 14], S43, 0xAB9423A7);
    b = global.II(b, c, d, a, x[k + 5], S44, 0xFC93A039);
    a = global.II(a, b, c, d, x[k + 12], S41, 0x655B59C3);
    d = global.II(d, a, b, c, x[k + 3], S42, 0x8F0CCC92);
    c = global.II(c, d, a, b, x[k + 10], S43, 0xFFEFF47D);
    b = global.II(b, c, d, a, x[k + 1], S44, 0x85845DD1);
    a = global.II(a, b, c, d, x[k + 8], S41, 0x6FA87E4F);
    d = global.II(d, a, b, c, x[k + 15], S42, 0xFE2CE6E0);
    c = global.II(c, d, a, b, x[k + 6], S43, 0xA3014314);
    b = global.II(b, c, d, a, x[k + 13], S44, 0x4E0811A1);
    a = global.II(a, b, c, d, x[k + 4], S41, 0xF7537E82);
    d = global.II(d, a, b, c, x[k + 11], S42, 0xBD3AF235);
    c = global.II(c, d, a, b, x[k + 2], S43, 0x2AD7D2BB);
    b = global.II(b, c, d, a, x[k + 9], S44, 0xEB86D391);
    a = global.AddUnsigned(a, AA);
    b = global.AddUnsigned(b, BB);
    c = global.AddUnsigned(c, CC);
    d = global.AddUnsigned(d, DD);
  }
  var temp = global.WordToHex(a) + global.WordToHex(b) + global.WordToHex(c) + global.WordToHex(d);
  return temp.toLowerCase();
}
global.RotateLeft = function(lValue, iShiftBits) {
  return (lValue << iShiftBits) | (lValue >>> (32 - iShiftBits));
}
global.AddUnsigned = function(lX, lY) {
  var lX4, lY4, lX8, lY8, lResult;
  lX8 = (lX & 0x80000000);
  lY8 = (lY & 0x80000000);
  lX4 = (lX & 0x40000000);
  lY4 = (lY & 0x40000000);
  lResult = (lX & 0x3FFFFFFF) + (lY & 0x3FFFFFFF);
  if (lX4 & lY4) {
    return (lResult ^ 0x80000000 ^ lX8 ^ lY8);
  }
  if (lX4 | lY4) {
    if (lResult & 0x40000000) {
      return (lResult ^ 0xC0000000 ^ lX8 ^ lY8);
    } else {
      return (lResult ^ 0x40000000 ^ lX8 ^ lY8);
    }
  } else {
    return (lResult ^ lX8 ^ lY8);
  }
}
global.F = function(x, y, z) {
  return (x & y) | ((~x) & z);
}
global.G = function(x, y, z) {
  return (x & z) | (y & (~z));
}
global.H = function(x, y, z) {
  return (x ^ y ^ z);
}
global.I = function(x, y, z) {
  return (y ^ (x | (~z)));
}
global.FF = function(a, b, c, d, x, s, ac) {
  a = global.AddUnsigned(a, global.AddUnsigned(global.AddUnsigned(F(b, c, d), x), ac));
  return global.AddUnsigned(global.RotateLeft(a, s), b);
}
global.GG = function(a, b, c, d, x, s, ac) {
  a = global.AddUnsigned(a, global.AddUnsigned(global.AddUnsigned(G(b, c, d), x), ac));
  return global.AddUnsigned(global.RotateLeft(a, s), b);
}
global.HH = function(a, b, c, d, x, s, ac) {
  a = global.AddUnsigned(a, global.AddUnsigned(global.AddUnsigned(H(b, c, d), x), ac));
  return global.AddUnsigned(global.RotateLeft(a, s), b);
}
global.II = function(a, b, c, d, x, s, ac) {
  a = global.AddUnsigned(a, global.AddUnsigned(global.AddUnsigned(I(b, c, d), x), ac));
  return global.AddUnsigned(global.RotateLeft(a, s), b);
}
global.ConvertToWordArray = function(string) {
  var lWordCount;
  var lMessageLength = string.length;
  var lNumberOfWords_temp1 = lMessageLength + 8;
  var lNumberOfWords_temp2 = (lNumberOfWords_temp1 - (lNumberOfWords_temp1 % 64)) / 64;
  var lNumberOfWords = (lNumberOfWords_temp2 + 1) * 16;
  var lWordArray = Array(lNumberOfWords - 1);
  var lBytePosition = 0;
  var lByteCount = 0;
  while (lByteCount < lMessageLength) {
    lWordCount = (lByteCount - (lByteCount % 4)) / 4;
    lBytePosition = (lByteCount % 4) * 8;
    lWordArray[lWordCount] = (lWordArray[lWordCount] | (string.charCodeAt(lByteCount) << lBytePosition));
    lByteCount++;
  }
  lWordCount = (lByteCount - (lByteCount % 4)) / 4;
  lBytePosition = (lByteCount % 4) * 8;
  lWordArray[lWordCount] = lWordArray[lWordCount] | (0x80 << lBytePosition);
  lWordArray[lNumberOfWords - 2] = lMessageLength << 3;
  lWordArray[lNumberOfWords - 1] = lMessageLength >>> 29;
  return lWordArray;
}
global.WordToHex = function(lValue) {
  var WordToHexValue = "", WordToHexValue_temp = "", lByte, lCount;
  for (lCount = 0; lCount <= 3; lCount++) {
    lByte = (lValue >>> (lCount * 8)) & 255;
    WordToHexValue_temp = "0" + lByte.toString(16);
    WordToHexValue = WordToHexValue + WordToHexValue_temp.substr(WordToHexValue_temp.length - 2, 2);
  }
  return WordToHexValue;
}
global.Utf8Encode = function(string) {
  var utftext = "";
  for (var n = 0; n < string.length; n++) {
    var c = string.charCodeAt(n);
    if (c < 128) {
      utftext += String.fromCharCode(c);
    } else if ((c > 127) && (c < 2048)) {
      utftext += String.fromCharCode((c >> 6) | 192);
      utftext += String.fromCharCode((c & 63) | 128);
    } else {
      utftext += String.fromCharCode((c >> 12) | 224);
      utftext += String.fromCharCode(((c >> 6) & 63) | 128);
      utftext += String.fromCharCode((c & 63) | 128);
    }
  }
 return utftext;
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
