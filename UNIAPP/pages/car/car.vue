<template>
	<view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		},
		onLoad() {
			let that=this
			uni.getStorage({
			    key: 'dengLu',
			    success: function (res) {
					that.dengLu=true
			    },
				fail() {
					that.dengLu=false
				}
			});
		}
	}
</script>

<style>

</style>
