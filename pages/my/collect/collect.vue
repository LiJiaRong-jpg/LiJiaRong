<template>
	<view>
		<view v-for="(key,index) in shuJi.items" style="clear: both;">
			<view style="float: left;width: 40%;">
				<image :src="key.img" mode="" style="width: 10rem;height: 10rem;"></image>
			</view>
			<view style="float: left;width: 40%;overflow: hidden;white-space: nowrap;text-overflow: ellipsis;">
				<text>{{key.title}}\n</text>
				<text style="color: #808080;">{{key.description}}\n</text>
				<text style="color: #808080;">累计销售{{key.sales}}件\n</text>
				<text style="text-decoration: line-through;">￥{{key.originalPrice/100}}</text>
				<text>￥{{key.price/100}}</text>
			</view>
			<view style="float: left;width: 20%;">
				<text class="iconfont" @click="quXiao(index)">&#xe62b;</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				shuJi: []
			}
		},
		methods: {
			quXiao(e) {
				let that=this
				uni.getStorage({
					key: 'dengLu',
					success(ee) {
						uni.request({
							url: 'http://192.168.1.84:8081/m.api', //仅为示例，并非真实接口地址。
							data: {
								_gp: 'collect',
								_mt: 'deleteCollect',
								spuId: that.shuJi.items[e].spuId
							},
							method: "POST",
							header: {
								"Content-type": "application/x-www-form-urlencoded",
								accessToken: ee.data
							},
							success() {
								that.load()
							}
						})
					}
				});
			},
			load() {
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success: function(res) {
						uni.request({
							url: 'http://192.168.1.84:8081/m.api', //仅为示例，并非真实接口地址。
							data: {
								_gp: 'collect',
								_mt: 'getCollectAll'
							},
							header: {
								"Content-type": "application/x-www-form-urlencoded",
								"accesstoken": res.data
							},
							success: (resd) => {
								that.shuJi = resd.data.data
							}
						});
					}
				});
			}
		},
		onShow() {
			this.load()
		}
	}
</script>

<style>

</style>
