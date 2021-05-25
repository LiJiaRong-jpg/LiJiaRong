<template>
	<view>
		<view class="uni-padding-wrap">
			<view class="page-section swiper">
				<view class="page-section-spacing">
					<swiper class="swiper" indicator-dots="false">
						<swiper-item v-for="(key,index) in shuJu.imgList">
							<image :src="key" mode="" style="width: 100%;" />
						</swiper-item>
					</swiper>
				</view>
			</view>
		</view>
		<view class="text">
			<text>{{shuJu.title}}\n</text>
			<text style="color: #FC4369;">￥{{shuJu.skuList[leiXing].price/100}}</text>
			<text style="text-decoration: line-through;color: #C0C0C0;"
				v-if="shuJu.price!=shuJu.originalPrice">￥{{shuJu.skuList[leiXing].originalPrice/100}}</text>
			<text>\n</text>
			<text style="width: 50%;display: inline-block;">销量：{{shuJu.sales}}</text>
			<text style="width: 50%;display: inline-block;">库存：{{shuJu.stock}}</text>
			<view style="width: 100%;height: 2.5rem;" @click="gM">
				<text style="width: 80%;float: left;">购买类型 {{shuJu.skuList[leiXing].title}}</text>
				<text class="iconfont" style="width: 10%;float: right;margint:0 5%;">&#xe61f;</text>
			</view>
			<view style="width: 100%;height: 2.5rem;" @click="yH">
				<text style="width: 20%;float: left;">优惠劵 </text>
				<text style="width: 60%;float: left;color: #FC4369;">领取优惠劵</text>
				<text class="iconfont" style="width: 10%;float: right;margint:0 5%;">&#xe61f;</text>
			</view>
			<view style="clear: both;">
				<text>配送费用 单笔购买满￥{{shuJu.freightTemplate.freightTemplateDO.defaultFreePrice / 100}}元免邮费</text>
			</view>
			<view v-if="shuJu.attributeList[0].value">
				<text>生产产地 {{shuJu.attributeList[0].value}}</text>
			</view>
			<view style="width: 100%;height: 5px;background-color: #C0C0C0;"></view>
			<view class="detail-desc">
				<view class="d-header">
					<text style="display: block;text-align: center;">图文详情</text>
				</view>
				<view v-for="(key,index) in shuJu.detail">
					<image :src="key" mode="" style="width: 100%;" @click="clickImg(index)"></image>
				</view>
			</view>
			<view v-if="xuang" style="top: 0;position: fixed;width: 100%;z-index: 999;">
				<view style="width: 100%;height: 30vh;background-color: rgba(0, 0, 0, 0.4);" @click="xuang=false">
				</view>
				<view
					style="width: 100%;height: 70vh;box-sizing: content-box;background-color: white;overflow-y: auto;">
					<view>
						<image :src="shuJu.img" mode=""
							style="width: 130px;height: 130px;float: left;overflow-y: hidden;"></image>
						<view style="float: left;display: contents;">
							<text style="color: #FC4369;">￥{{shuJu.skuList[leiXing].price}}</text>
							<text>\n库存：{{shuJu.skuList[leiXing].stock}}\n已选：{{shuJu.skuList[leiXing].title}}</text>
						</view>
					</view>
					<text style="clear: both;padding-left: 5%;">\n规格</text>
					<view>
						<text v-for="(key,index) in shuJu.skuList" :class="leiXing==index?'guige':'Guige'"
							@click="guige(index)">
							{{key.title}}
						</text>
					</view>
					<text>数量\n</text>
					<view class="num" style="line-height: 4rem;">
						<view @click="number(0)">-</view>
						<view>{{num}}</view>
						<view @click="number(1)">+</view>
					</view>
					<view @click="addCart"
						style="text-align: center;background-color: #FC4369;width: 90%;margin: auto;border-radius: 15px;color: white;">
						<text>完成</text>
					</view>
				</view>
			</view>
		</view>
		<!-- 底部操作菜单 -->
		<view class="page-bottom">
			<navigator url="/pages/index/index" open-type="switchTab" class="p-b-btn" style="width: 14%;">
				<text class="iconfont">&#xe60f;</text>
				<text>\n首页</text>
			</navigator>
			<navigator url="/pages/car/car" open-type="switchTab" class="p-b-btn" style="width: 16%;">
				<text class="iconfont">&#xe609;</text>
				<text>\n购物车</text>
			</navigator>
			<view class="p-b-btn" :class="{active: goods.collect}" style="width: 14%;" v-if="!shouChang" @click="shou">
				<text class="iconfont">&#xe619;</text>
				<text>\n收藏</text>
			</view>
			<view class="p-b-btn" :class="{active: goods.collect}" style="width: 14%;" v-if="shouChang" @click="shou">
				<text class="iconfont" style="color: #FC4369;">&#xe619;</text>
				<text style="color: #FC4369;">\n收藏</text>
			</view>
			<view class="action-btn-group">
				<view type="primary" class=" action-btn no-border buy-now-btn" @click="buy(0)">立即购买</view>
				<view type="primary" class=" action-btn no-border add-cart-btn" @click="buy(1)">加入购物车</view>
			</view>
		</view>
		<view style="width: 100%;height: 100vh;background-color: rgba(0,0,0,0.6);position: fixed;z-index: 99999;top: 0;"
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
		<view v-if="you" style="top: 0;position: fixed;width: 100%;z-index: 999;">
			<view style="width: 100%;height: 30vh;background-color: rgba(0, 0, 0, 0.4);" @click="you=false">
			</view>
			<view style="width: 100%;height: 70vh;box-sizing: content-box;background-color: white;
					overflow-y: auto;">
				<view v-for="(key,index) in youHui" style="border-bottom: #808080 1px solid;width: 100%;height: 4rem;">
					<view style="width: 75%;float: left;">
						<text>{{key.title}}\n</text>
						<text v-if="key.gmtEnd" style="font-size: 14px;">在{{key.gmtEnd}}前有效。
							可领{{key.limit}}张，已领{{key.nowCount}}张\n</text>
						<text v-if="!key.gmtEnd"
							style="font-size: 14px;">在领取后{{key.days}}天内有效。可领{{key.limit}}张，已领{{key.nowCount}}张</text>
					</view>
					<view style="width: 25%;float: right;">
						<text style="display: block;text-align: center;">￥{{key.discount / 100.0}}\n</text>
						<text style="display: block;text-align: center;">满￥{{key.min / 100.0}}可用</text>
					</view>
					<view style="width: 100%;height: 2rem;">
						<text>{{key.categoryTitle?'限' + key.categoryTitle + '可用': '全品类可用'}}</text>
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
				dengLu: true, //是否登录
				acc: null,
				shouChang: false,
				shuJu: [],
				leiXing: 0,
				num: 1,
				xuang: false,
				gou: 0, //0为买，1为购物车
				youHui: [],
				you: false,
				ding: {
					"skuList": [{
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
					}],
					"totalOriginalPrice": null,
					"totalPrice": null,
					"mono": "",
					"takeWay": "buy",
					"freightPrice": 0,
					"addressId": null
				}
			}
		},
		methods: {
			gM() {
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success() {
						that.xuang = true
					},
					fail() {
						that.dengLu = true
					}
				});
			},
			yH() {
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success() {
						that.you = true
					},
					fail() {
						that.dengLu = true
					}
				});
			},
			QU() {
				this.dengLu = false
			},
			DENGLU() {
				uni.navigateTo({
					url: "../../../../my/role/role"
				})
				this.dengLu = false
			},
			shou() {
				let that = this
				if (that.shouChang == true) {
					uni.getStorage({
						key: 'id',
						success(e) {
							uni.getStorage({
								key: 'dengLu',
								success(ee) {
									uni.request({
										url: 'http://192.168.1.54:8081/m.api', //仅为示例，并非真实接口地址。
										data: {
											_gp: 'collect',
											_mt: 'deleteCollect',
											spuId: e.data
										},
										method: "POST",
										header: {
											"Content-type": "application/x-www-form-urlencoded",
											accessToken: ee.data
										},
										success: (res) => {
											that.shouChang = false
										}
									})
								}
							});
						}
					});
				} else {
					uni.getStorage({
						key: 'id',
						success(e) {
							uni.getStorage({
								key: 'dengLu',
								success(ee) {
									uni.request({
										url: 'http://192.168.1.54:8081/m.api', //仅为示例，并非真实接口地址。
										data: {
											_gp: 'collect',
											_mt: 'addCollect',
											spuId: e.data
										},
										method: "POST",
										header: {
											"Content-type": "application/x-www-form-urlencoded",
											accessToken: ee.data
										},
										success: (res) => {
											that.shouChang = true
										}
									})
								}
							});
						}
					});
				}
				uni.getStorage({
					key: 'dengLu',
					fail() {
						that.xuang = false
						that.dengLu = true
					}
				});
			},
			clickImg(e) {
				let that = this
				uni.previewImage({
					urls: that.shuJu.detail, //需要预览的图片http链接列表，多张的时候，url直接写在后面就行了
					current: e
				})
			},
			preview(src, e) {
				// do something
			},
			buy(index) {
				this.xuang = true
				this.gou = index
				let that = this
				uni.getStorage({
					key: 'dengLu',
					fail() {
						that.xuang = false
						that.dengLu = true
					}
				});
			},
			guige(index) {
				this.leiXing = index
			},
			number(e) {
				if (e == 0) {
					if (this.num > 1) {
						this.num--
						this.ding.skuList[0].num = this.num
					}
				} else {
					this.num++
					this.ding.skuList[0].num = this.num
				}
			},
			addCart() {
				this.xuang = false
				if (this.gou == 0) {
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
									that.ding.skuList[0].id = resd.data.data.id
								}
							});
						}
					});
					this.ding.skuList[0].categoryId = this.shuJu.categoryId
					this.ding.skuList[0].categoryIdList = this.shuJu.categoryIds
					this.ding.skuList[0].spuImg = this.shuJu.img
					this.ding.skuList[0].skuId = this.shuJu.skuList[this.leiXing].id
					this.ding.skuList[0].skuTitle = this.shuJu.skuList[this.leiXing].title
					this.ding.skuList[0].originalPrice = this.shuJu.skuList[this.leiXing].originalPrice
					this.ding.skuList[0].price = this.shuJu.skuList[this.leiXing].price
					this.ding.skuList[0].vipPrice = this.shuJu.skuList[this.leiXing].vipPrice
					this.ding.skuList[0].stock = this.shuJu.skuList[this.leiXing].stock
					this.ding.skuList[0].spuId = this.shuJu.skuList[this.leiXing].spuId
					this.ding.skuList[0].title = this.shuJu.title
					this.ding.totalPrice = this.shuJu.skuList[this.leiXing].price
					this.ding.totalOriginalPrice = this.shuJu.skuList[this.leiXing].originalPrice
					uni.setStorage({
						key: 'toke',
						data: this.ding
					})
					uni.navigateTo({
						url: "../dingDang/dingDang"
					})
				} else {
					let that = this
					uni.getStorage({
						key: 'dengLu',
						success: function(res) {
							uni.request({
								url: 'http://192.168.1.54:8081/m.api', //仅为示例，并非真实接口地址。
								data: {
									_gp: 'cart',
									_mt: 'addCartItem',
									skuId: that.shuJu.skuList[that.leiXing].id,
									num: that.num
								},
								header: {
									"Content-type": "application/x-www-form-urlencoded",
									"accesstoken": res.data
								},
								success: (resd) => {
									that.xuang = false
								}
							});
						}
					});
				}
			}
		},
		onShow() {
			let that = this
			uni.getStorage({
				key: 'dengLu',
				success: function(res) {
					that.acc = res.data
					that.dengLu = false

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
			})
			uni.getStorage({
				key: 'id',
				success(e) {
					uni.request({
						url: 'http://192.168.1.54:8081/m.api', //仅为示例，并非真实接口地址。
						data: {
							_gp: 'goods',
							_mt: 'getGoods',
							spuId: e.data
						},
						method: "POST",
						header: {
							"Content-type": "application/x-www-form-urlencoded; charset=UTF-8",
							'accessToken': that.acc
						},
						success: (res) => {
							res = res.data.data
							that.shuJu = res
							let arr = []
							let array = res.detail.split('"')
							let regex = (/http|jpg|png|gif/i);
							for (let a in array) {
								let b = regex.test(array[a])
								if (b == true) {
									arr.push(array[a])
								}
							}
							that.shuJu.detail = arr
							if (that.shuJu.imgList.length == 0) {
								that.shuJu.imgList.push(that.shuJu.img)
							}
							uni.getStorage({
								key: 'dengLu',
								success(ee) {
									uni.request({
										url: 'http://192.168.1.54:8081/m.api', //仅为示例，并非真实接口地址。
										data: {
											_gp: 'collect',
											_mt: 'getCollectBySpuId',
											spuId: e.data
										},
										method: "POST",
										header: {
											"Content-type": "application/x-www-form-urlencoded",
											accessToken: ee.data
										},
										success: (res) => {
											that.shouChang = res.data.data
										}
									})
									uni.request({
										url: 'http://192.168.1.54:8081/m.api', //仅为示例，并非真实接口地址。
										data: {
											_gp: 'coupon',
											_mt: 'getObtainableCoupon'
										},
										method: "POST",
										header: {
											"Content-type": "application/x-www-form-urlencoded",
											accessToken: ee.data
										},
										success: (res) => {
											that.youHui = res.data.data
										}
									})
								}
							})
						}
					});
				}
			});
		}
	}
</script>

<style>
	.uni-padding-wrap swiper-item>view {
		width: 100%;
		height: 6.5rem;
	}

	swiper {
		height: calc(99vw*9/12)
	}

	.text text {
		line-height: 2.5rem;
	}

	.guige {
		color: #FC4369;
		background-color: #fbebee;
		padding: 5px;
		border-radius: 10px;
		margin: 10px;
	}

	.Guige {
		padding: 5px;
		background-color: #C0C0C0;
		color: #000000;
		background-color: #C0C0C0;
		border-radius: 10px;
		margin: 10px;
	}

	.num>view {
		display: initial;
		padding: 10px;
		background-color: #C0C0C0;
		margin: 5px;
		margin-top: 1rem;
		margin-bottom: 2rem;
	}

	.rich-img {
		width: 100%;
		height: auto;
		margin: 0;
		padding: 0;
		line-height: 0px;
	}

	.rich-img>weixin-parse-template {
		width: 100%;
		display: flow-root;
	}

	.page-bottom {
		position: fixed;
		bottom: 10px;
		width: 96%;
		height: 4rem;
		border-radius: 5px;
		margin-left: 2%;
		background: rgba(255, 255, 255, 0.9);
		box-shadow: 0 0 15px 0 rgb(0 0 0 / 50%);
	}

	.page-bottom .action-btn-group {
		background: -webkit-linear-gradient(left, #ffac30, #fa436a, #F56C6C);
		border-radius: 10px;
		line-height: 3rem;
		margin-top: 0.2rem;
		margin-left: -40rpx;
		color: white;
		width: 53%;
		float: right;
		margin-right: 4rpx;
	}

	.page-bottom .action-btn-group>view:first-child {
		float: left;
		width: 49%;
		padding: 5px 0;
		border-right: #808080 1px solid;
		text-align: center;
	}

	.page-bottom .action-btn-group>view:last-child {
		float: left;
		width: 50%;
		padding: 5px 0;
		text-align: center;
	}

	.page-bottom>navigator,
	.page-bottom>view {
		float: left;
	}

	.p-b-btn {
		margin: 3px;
		text-align: center;
	}
</style>
