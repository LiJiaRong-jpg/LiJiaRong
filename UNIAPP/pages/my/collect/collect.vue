<template>
	<view>
		{{res}}
	</view>
</template>

<script>
	export default {
		data() {
			return {
				res:null
			}
		},
		methods: {
			
		},
		onLoad() {
			let that = this
			uni.request({
				url: 'http://192.168.1.84:8081/m.api', //仅为示例，并非真实接口地址。
				data: {
					_gp: 'collect',
					_mt: 'getCollectAll'
				},
				header: {
					"Content-type": "application/x-www-form-urlencoded",
					accesstoken: '5F8A81E49B204B19AC9D807AF76FE472'
				},
				success: (res) => {
					console.log(res.data);
					that.res=res.data.errmsg
				}
			});
		}
	}
</script>

<style>

</style>
