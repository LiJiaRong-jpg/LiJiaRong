<template>
	<view>
		<view v-for="(key,index) in shuJu">
			<view>

			</view>
			<view class="img">
				<image :src="key.spuImg" mode=""></image>
			</view>
			<view class="buttom">
				<text>{{key.title}}\n</text>
				<text>{{key.skuTitle}}\n</text>
				<text style="text-decoration: line-through;">￥{{key.originalPrice/100}}</text>
				<text>￥{{key.price/100}}\n</text>
				<button @click="change(0,index)">-</button>
				<button disabled="true">{{key.num}}</button>
				<button @click="change(1,index)">+</button>
			</view>
			<view>

			</view>
		</view>


		<view style="width: 100%;height: 100vh;background-color: rgba(0,0,0,0.6);position: fixed;z-index: 999;top: 0;"
			v-if="dengLu">
			<view style="width: 40vh;height: 20vh;background-color: white;margin: auto;margin-top: 40vh;">
				<view style="margin: auto;margin-top: 1rem;display: table;line-height: 6vh;">登录提示</view>
				<view style="margin: auto;color: #808080;display: table;line-height: 6vh;">您尚未登录，是否立即登录？</view>
				<view
					style="border-top: #808080 solid 2rpx;margin-top:2vh;width: 50%;float: left;text-align: center;line-height: 6vh;border-right: #808080 solid 2rpx;box-sizing: border-box;"
					@click="QU">取消</view>
				<view
					style="border-top: #808080 solid 2rpx;margin-top:2vh;width: 50%;float: left;text-align: center;line-height: 6vh;color: #007AFF;"
					@click="DENGLU">登录</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				shuJu: [],
				dengLu: true, //是否登录
			}
		},
		methods: {
			QU() {
				this.dengLu = false
			},
			DENGLU() {
				uni.navigateTo({
					url: "../my/role/role"
				})
				this.dengLu = false
			},
			change(e, i) {
				if (e == 0) {
					e = this.shuJu[i].num-1
				} else {
					e = this.shuJu[i].num+1
				}
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success: function(res) {
						uni.request({
							url: 'http://192.168.1.84:8081/m.api',
							data: {
								_gp: 'cart',
								_mt: 'updateCartItemNum',
								num: e,
								cartId: that.shuJu[i].id
							},
							method: "POST",
							header: {
								"Content-type": "application/x-www-form-urlencoded",
								accessToken: res.data
							},
							success: (resd) => {
								that.load()
							}
						});
					}
				});
			},
			load(){
				let that = this
			uni.getStorage({
				key: 'dengLu',
				success: function(res) {
					uni.request({
						url: 'http://192.168.1.84:8081/m.api',
						data: {
							_gp: 'cart',
							_mt: 'getCartList'
						},
						method: "POST",
						header: {
							"Content-type": "application/x-www-form-urlencoded",
							accessToken: res.data
						},
						success: (resd) => {
							that.shuJu = resd.data.data
							console.log(resd)
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
.buttom>button{
	display: inline-block;
}
.buttom{
		width: 40%;
		overflow:hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
	float: left;
}
.img{
	float: left;
}
.img>image{
	width: 30vh;
    height: 30vh;
	
}
</style>
