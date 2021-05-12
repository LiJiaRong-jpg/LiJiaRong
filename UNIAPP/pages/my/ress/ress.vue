<template>
	<view>
		<view v-for="(key,index) in DATA" style="width: 100%;height: 3rem;clear: both;">
			<text>{{key.consignee}} {{key.phone}}\n</text>
			<text style="border: 1px #fe5677 solid;color: #fe5677;" v-if="key.defaultAddress==1">默认</text>
			<text>{{key.province}} {{key.city}} {{key.county}} {{key.address}}</text>
			<text class="iconfont" style="float: right;margin-right: 10rpx;" @click="xiuGai({index})">&#xe611;</text>
		</view>



		<view style="width: 80%;background-color: #fc4369;margin-left: 5%;border-radius: 1rem;
			padding:0 1rem ;line-height: 2rem;position: fixed;bottom: 10px;" @click="addRess">
			<text style="display: table;margin: auto;">新增地址</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				res: null,
				DATA: []
			}
		},
		methods: {
			addRess() {
				uni.navigateTo({
					url: "./addRess/addRess?gp='address'&mt='addAddress'"
				})
			},
			xiuGai(e) {
				let a =this.DATA[e.index]
				uni.navigateTo({
					url: "./addRess/addRess?gp=address&mt=updateAddress&consignee="+a.consignee+
					"&phone="+a.phone+"&province="+a.province+"&address="+a.address+"&city="+a.city+
					"&county="+a.county+"&id="+a.id+""
				})
			}
		},
		onShow() {
			let that = this
			uni.getStorage({
				key: 'dengLu',
				success: function(res) {
					uni.request({
						url: 'http://192.168.1.84:8081/m.api', //仅为示例，并非真实接口地址。
						data: {
							_gp: 'address',
							_mt: 'getAllAddress'
						},
						header: {
							"Content-type": "application/x-www-form-urlencoded",
							"accesstoken": res.data
						},
						success: (resd) => {
							that.DATA = resd.data.data
						}
					});
				}
			});
		}
	}
</script>

<style>

</style>
