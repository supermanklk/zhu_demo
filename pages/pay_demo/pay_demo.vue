<template>
	<view>
		<button @click="pay()" type="primary">支付测试</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
		methods:{
			pay() {
				console.log('点击了支付');
				let time = new Date().getTime();
				uni.request({
					// url: 'http://crm.binbin.aiyongbao.com/Zhu/md5binbin',
					url: global.host + 'Zhu/md5binbin',
					method: 'GET',
					data: {
						out_trade_no : time + '',
						total_fee : '1',
						openid : 'oPreH5IKKdGV-EVnvML-I-eKnTYQ',
						appid : 'wx08c77d4e3bb67d91',
						mch_id : '1529227481',
					},
					success: res => {
						console.log('第一次签名',res);
						let prepay_id = res.data.prepay_id;
						uni.request({
							// url: 'http://crm.binbin.aiyongbao.com/Zhu/secondmd5',
							url: global.host + 'Zhu/secondmd5',
							method: 'GET',
							data: {
								appId : 'wx08c77d4e3bb67d91',
								// nonce_str
								nonceStr : '5K8264ILTKCH16CQ2502SI8ZNMTM67VS',
								package : 'prepay_id=' + prepay_id,
								timeStamp : time + ''
							},
							success: res => {
								console.log('再次签名',res);
								console.log('prepay_id',prepay_id);
									uni.requestPayment({
									provider: 'wxpay',
									timeStamp: time + '',
									nonceStr: '5K8264ILTKCH16CQ2502SI8ZNMTM67VS',
									package: 'prepay_id=' + prepay_id,
									signType: 'MD5',
									paySign: res.data,
									success: function (res) {
										console.log('支付的返回结果success',res);
										if(res.errMsg == 'requestPayment:ok') {
											uni.showToast({
												title: '支付成功',
												duration: 2000
											});
										}
										console.log('success:' + JSON.stringify(res));
									},
									fail: function (err) {
										uni.showToast({
											title: '支付失败',
											duration: 2000,
											icon : 'none'
										});
										console.log('支付的返回结果err',res);
										console.log('fail:' + JSON.stringify(err));
									}
								});
							},
							fail: () => {},
							complete: () => {}
						});
					},
					fail: () => {},
					complete: () => {}
				});
				
			
				
				

				

			}
		}
		
	}
</script>

<style>

</style>
