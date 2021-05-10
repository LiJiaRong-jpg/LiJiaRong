<template>
	<view>
		<view style="width: 30%;">
			<view v-for="(key , index) in DATA" data-id="{{index}}" :class="clas==index?'num':''">
				<text style="line-height: 3rem;text-align: center;display: block;">{{key.title}}</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				DATA: [],
				clas:0
			}
		},
		methods: {

		},
		onLoad() {
			let that = this
			uni.request({
				url: 'http://192.168.1.84:8081/m.api',
				data: {
					_gp: 'category',
					_mt: 'categoryList'
				},
				method: "GET",
				header: {
					"Content-type": "application/x-www-form-urlencoded"
				},
				success: (res) => {
					that.DATA = res.data.data
					console.log(that.DATA)
				}
			});
		}
	}
</script>

<style>
.num{
	background-color: #808080;
}
</style>
