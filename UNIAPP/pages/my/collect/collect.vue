<template>
	<view>
		{{res}}
	</view>
</template>

<script>
	export default {
		data() {
			return {
				res: null
			}
		},
		methods: {

		},
		onLoad() {
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
							that.res = resd.data.errmsg
						}
					});
				}
			});
		}
	}
</script>

<style>

</style>
