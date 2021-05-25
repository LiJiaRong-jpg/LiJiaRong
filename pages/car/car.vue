<template>
	<view>
		<view v-if="shuJu.length==0" style="width: 100%;">
			<view>
			<image src="../../static/emptyCart.jpeg" mode="" style="width: 60%;margin-left: 20%;margin-top: 20%;">
			</image>
		</view>
		<view style="display: table;margin: auto;">
			<text style="float: left;">空空如也</text>
			<navigator url="../index/index" open-type="switchTab" class="p-b-btn" style="color: #FC4369;float: left;margin-left: 10px;">
				<text>随便看看></text>
			</navigator>
		</view>
		</view>
		<view v-for="(key,index) in shuJu" style="clear: both;" v-if="shuJu.length>0">
			<view style="float: left;margin-left: 5%;float: left;">
				<label>
					<checkbox :checked="xZ[index]" @click="check(index)" />
				</label>
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
				<text class="iconfont" @click="dele(index)">&#xe62b;</text>
			</view>
		</view>
		<view style="width: 100%;height: 5rem;clear: both;"></view>
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
		<view v-if="shuJu.length>0"
			style="width: 100%;display: flow-root;background-color: white;position: fixed;bottom: 2vh;padding-top: 20rpx;">
			<view style="width: 80%;height: 3rem;margin-left: 5%;box-shadow: 0 0 13px 0 rgb(0 0 0 / 50%);
					border-radius: 10px;padding: 0.5rem 5%;">
				<label style="float: left;">
					<checkbox :checked="zXX" @click="zXZ" />
					<text v-if="zXX"
						style="background-color: #c0c4cb;position: relative;top: 4rpx;left: -10rpx;color: white;padding: 0 10rpx;border-radius: 0 20rpx 20rpx 0;"
						@click="qingKong">清空</text>
				</label>
				<view style="float: right;">
					<text style="float: left;">￥{{much[0]}}\n共{{much[1]}}件</text>
					<view @click="jieShuang"
						style="padding: 10px 15px;color: white;background-color: #FC4369;float: left;border-radius: 30px;margin-left: 10px;">
						去结算
					</view>
				</view>
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
				xZ: [],
				much: [0, 0],
				zXX: true,
				ding: {
					"skuList": [],
					"totalOriginalPrice": null,
					"totalPrice": null,
					"mono": "",
					"takeWay": "cart",
					"freightPrice": 0,
					"addressId": null
				}
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
					e = this.shuJu[i].num - 1
				} else {
					e = this.shuJu[i].num + 1
				}
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success: function(res) {
						uni.request({
							url: 'http://192.168.1.54:8081/m.api',
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
			load() {
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success: function(res) {
						that.dengLu = false
						uni.request({
							url: 'http://192.168.1.54:8081/m.api',
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
								for (let i in that.shuJu) {
									that.xZ.push(true)
								}
								that.zong()
							}
						});
					},
					fail() {
						that.dengLu = true
					}
				});
			},
			dele(e) {
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success(ee) {
						uni.request({
							url: 'http://192.168.1.54:8081/m.api', //仅为示例，并非真实接口地址。
							data: {
								_gp: 'cart',
								_mt: 'removeCartItem',
								cartId: that.shuJu[e].id
							},
							method: "POST",
							header: {
								"Content-type": "application/x-www-form-urlencoded",
								accessToken: ee.data
							},
							success: (res) => {
								uni.showToast({
									title: res.data.errmsg,
									duration: 2000
								});
								that.load()
							}
						})
					}
				});
			},
			check(e) { //改变选择情况
				this.xZ[e] = !this.xZ[e]
				this.zong()
			},
			zong() { //总价
				this.much[0] = 0
				this.much[1] = 0
				this.zXX = true
				for (let i in this.shuJu) {
					if (this.xZ[i] == true) {
						this.much[0] += this.shuJu[i].price / 100 * this.shuJu[i].num
						this.much[1] += this.shuJu[i].num
					} else {
						this.zXX = false
					}
				}
			},
			zXZ() {
				this.zXX = !this.zXX
				for (let i in this.shuJu) {
					this.xZ[i] = this.zXX
				}
				this.zong()
			},
			qingKong() {
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success(ee) {
						uni.request({
							url: 'http://192.168.1.54:8081/m.api', //仅为示例，并非真实接口地址。
							data: {
								_gp: 'cart',
								_mt: 'removeCartAll'
							},
							method: "POST",
							header: {
								"Content-type": "application/x-www-form-urlencoded",
								accessToken: ee.data
							},
							success: (res) => {
								that.load()
								uni.showToast({
									title: res.data.errmsg,
									duration: 2000
								});
							}
						})
					}
				});
			},
			jieShuang() {
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success: function(res) {
						uni.request({
							url: "http://192.168.1.54:8081/m.api", //仅为示例，并非真实接口地址。
							data: {
								_gp: 'address',
								_mt: 'getDefAddress'
							},
							method: "POST",
							header: {
								"Content-type": "application/x-www-form-urlencoded",
								"accesstoken": res.data
							},
							success: (resd) => {
								that.ding.addressId = resd.data.data.id
							}
						});
					}
				});

				let arr = []

				for (let i in this.shuJu) {
					if (this.xZ[i] == true) {

						arr.push(this.shuJu[i].id)

						let object = {
							"skuId": null,
							"num": 1,
							"title": null,
							"originalPrice": null,
							"price": null,
							"vipPrice": null,
							"skuTitle": null,
							"spuImg": null,
							"stock": null,
							"spuId": null,
							"categoryId": null,
							"categoryIdList": null
						}
						object.categoryId = this.shuJu[i].categoryId
						object.categoryIdList = this.shuJu[i].categoryIdList
						object.num = this.shuJu[i].num
						object.originalPrice = this.shuJu[i].originalPrice
						object.price = this.shuJu[i].price
						object.skuId = this.shuJu[i].skuId
						object.skuTitle = this.shuJu[i].skuTitle
						object.spuId = this.shuJu[i].spuId
						object.spuImg = this.shuJu[i].spuImg
						object.stock = this.shuJu[i].stock
						object.vipPrice = this.shuJu[i].vipPrice
						object.title = this.shuJu[i].title

						this.ding.totalPrice += object.price * object.num
						this.ding.totalOriginalPrice += object.originalPrice * object.num
						this.ding.skuList.push(object)
					}
				}
				uni.setStorage({
					key: 'toke',
					data: this.ding
				})

				uni.setStorage({
					key: 'deleCreate',
					data: arr
				})

				this.ding = {
					"skuList": [],
					"totalOriginalPrice": null,
					"totalPrice": null,
					"mono": "",
					"takeWay": "buy",
					"freightPrice": 0,
					"addressId": null
				}
				uni.navigateTo({
					url: "../index/input/souSuo/dingDang/dingDang"
				})
			}
		},
		onShow() {
			this.load()
		}
	}
</script>

<style>
	.buttom>button {
		display: inline-block;
	}

	.buttom {
		width: 35%;
		overflow: hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
		float: left;
	}

	.img {
		float: left;
	}

	.img>image {
		width: 30vh;
		height: 30vh;

	}
</style>
