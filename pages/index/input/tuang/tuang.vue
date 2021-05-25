<template>
	<view>
		<view v-for="(key , index) in shuJi.items" style="clear: both;">
			<view><image :src="key.img" style="width: 10rem;height: 10rem;float: left;"></image></view>
			<view>
				<text>{{key.title}}\n</text>
				<progress :percent="100 * item.alreadyBuyNumber / item.minimumNumber" activeColor="#fa436a" active stroke-width="6" style="width: 8rem;float: left;margin-top: 14rpx;"/>
				<text>已{{key.minimumNumber}}人成团\n</text>
				<text>已拼团{{key.sales}}件\n</text>
				<text style="text-decoration: line-through;color: #808080;">￥{{key.originalPrice/1000}}</text>
				<text style="color: #FC4369;">￥{{(key.originalPrice-key.price)/1000}}</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				page:1,
				pageSize:10,
				shuJi:[]
			}
		},
		methods: {
			
		},
		onShow(){
			let that=this
			uni.request({
				url: 'http://192.168.1.54:8081/m.api',
				data: {
					_gp: 'groupshop',
					_mt: 'getGroupShopPage',
					page:that.page,
					pageSize:that.pageSize
				},
				method: "POST",
				header: {
					"Content-type": "application/x-www-form-urlencoded"
				},
				success: (res) => {
					that.shuJi=res.data.data
				}
			});
		}
	}
</script>

<style>

</style>
