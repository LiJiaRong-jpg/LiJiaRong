<template>
	<view>

		<view style="height: 30vh;overflow: hidden;">
			<image src="../../static/user-bg.jpeg" style="width: 100%;position: absolute;z-index: -1;" mode=""></image>
			<view style="margin-left: 5vh;margin-top: 10vh;">
				<view style="float: left;">
					<image :src="resd.avatarUrl" mode=""
						style="width: 15vh;height: 15vh;position: absolute;border-radius: 50%;border: white solid 5px;">
					</image>
				</view>
				<view
					style="float: left;position: absolute;line-height: 15vh;margin-left: 18vh;overflow:hidden;white-space:nowrap;text-overflow:ellipsis;width: 40vh;">
					<text>{{resd.nickname}}</text>
				</view>
			</view>
			<view style="width: 100%;position: absolute;">
				<view style="padding: 0 20rpx;width: 90%;margin: auto;background-color: #45433a;
					color: #ddb473;margin-top: 7.8rem;border-radius: 1rem 1rem 0 0;display: flow-root;">
					<view style="float: left;width: 40%;">
						<text class="iconfont" style="color: #ddb473;">&#xe623;</text>
						<text>普通会员</text>
					</view>
					<view style="float: right;width: 60%;">
						<image src="../../static/vip-card-bg.png" mode="" style="width: 100%;"></image>
					</view>
				</view>
			</view>
		</view>


		<view class="container" :style="[{
    transform: coverTransform,
    transition: coverTransition,
    top:top
   }]" @touchstart="coverTouchstart" @touchmove="coverTouchmove" @touchend="coverTouchend"
			style="background-color: white;">
			<view class="order-section">
				<view class="order-item" @click="QUAN(0)">
					<view><text class="iconfont"
							style="color: #fc4369;text-align: center;width: 100%;display: inline-block;">&#xe61d;</text>
					</view>
					<view><text style="text-align: center;width: 100%;display: inline-block;">全部订单</text></view>
				</view>
				<view class="order-item" @click="QUAN(1)">
					<view><text class="iconfont"
							style="color: #fc4369;text-align: center;width: 100%;display: inline-block;">&#xe617;</text>
					</view>
					<view><text style="text-align: center;width: 100%;display: inline-block;">待付款</text></view>
				</view>
				<view class="order-item" @click="QUAN(3)">
					<view><text class="iconfont"
							style="color: #fc4369;text-align: center;width: 100%;display: inline-block;">&#xe61e;</text>
					</view>
					<view><text style="text-align: center;width: 100%;display: inline-block;">待收货</text></view>
				</view>
				<view class="order-item" @click="QUAN(5)">
					<view><text class="iconfont"
							style="color: #fc4369;text-align: center;width: 100%;display: inline-block;">&#xe618;</text>
					</view>
					<view><text style="text-align: center;width: 100%;display: inline-block;">退款售后</text></view>
				</view>
			</view>

			<view id="di">
				<view style="border: 0;height: 6rem;" v-if="fenYe.length>0">
					<view>
						<text class="iconfont" style="color: #4CD964;margin-left: 1rem;float: left;">&#xe62c;</text>
						<text>浏览历史</text>
					</view>
					<scroll-view scroll-x class="h-list">
						<image v-for="(key, index ) in fenYe" :key="index" @longpress="long(index)"
							@click="clickFeng(index)" :src="key.spuImg"
							style="display: inline-block;width: 5rem;height: 5rem;"></image>
					</scroll-view>
				</view>
				<view @click="diZhi">
					<text class="iconfont" style="color: #4CD964;">&#xe612;</text>
					<text>地址管理</text>
					<text class="iconfont">&#xe61f;</text>
				</view>
				<view @click="collect">
					<text class="iconfont" style="color: #007AFF;">&#xe619;</text>
					<text>我的收藏</text>
					<text class="iconfont">&#xe61f;</text>
				</view>
				<view>
					<text class="iconfont" style="color: #e27472;">&#xe620;</text>
					<text>在线客服</text>
					<text class="iconfont">&#xe61f;</text>
				</view>
				<view @click="profile">
					<text class="iconfont" style="color: #f8b4f3;">&#xe614;</text>
					<text>个人资料</text>
					<text class="iconfont">&#xe61f;</text>
				</view>
				<view @click="about">
					<text class="iconfont" style="color: #ef8840;">&#xe621;</text>
					<text>关于</text>
					<text class="iconfont">&#xe61f;</text>
				</view>
				<view>
					<text class="iconfont" style="color: #e27472;">&#xe622;</text>
					<text>退出登录</text>
					<text class="iconfont">&#xe61f;</text>
				</view>
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

		<view style="width: 100%;height: 100vh;background-color: rgba(0,0,0,0.6);position: fixed;z-index: 999;top: 0;"
			v-if="dele">
			<view style="width: 40vh;height: 20vh;background-color: white;margin: auto;margin-top: 40vh;">
				<view style="margin: auto;margin-top: 1rem;display: table;line-height: 6vh;">删除？</view>
				<view style="margin: auto;color: #808080;display: table;line-height: 6vh;">您确定要删除此足迹吗？</view>
				<view
					style="border-top: #808080 solid 2rpx;margin-top:2vh;width: 50%;float: left;text-align: center;line-height: 6vh;border-right: #808080 solid 2rpx;box-sizing: border-box;"
					@click="dele=false">取消</view>
				<view
					style="border-top: #808080 solid 2rpx;margin-top:2vh;width: 50%;float: left;text-align: center;line-height: 6vh;color: #007AFF;"
					@click="deleteFeng">删除</view>
			</view>
		</view>
	</view>
</template>

<script>
	let startY = 0,
		moveY = 0,
		pageAtTop = true;
	export default {
		props: {
			top: {
				type: String,
				default: () => '30%'
			},
		},
		data() {
			return {
				coverTransform: 'translateY(0px)',
				coverTransition: '0s',
				moving: false,
				dengLu: true, //是否登录
				dele: false, //是否删除足迹
				resd: {
					nickname: '未登录',
					avatarUrl: ''
				}, //数据
				fenYe: null,
				num: 0
			}
		},
		methods: {
			about() {
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success: function(res) {
						uni.navigateTo({
							url: './about/about',
						})
					},
					fail() {
						that.dengLu = true
					}
				})
			},
			coverTouchstart(e) {
				if (pageAtTop === false) {
					return;
				}
				this.coverTransition = 'transform .1s linear';
				startY = e.touches[0].clientY;
			},
			coverTouchmove(e) {
				moveY = e.touches[0].clientY;
				let moveDistance = moveY - startY;
				if (moveDistance < 0) {
					this.moving = false;
					return;
				}
				this.moving = true;
				if (moveDistance >= 80 && moveDistance < 100) {
					moveDistance = 80;
				}

				if (moveDistance > 0 && moveDistance <= 80) {
					this.coverTransform = `translateY(${moveDistance}px)`;
				}
			},
			coverTouchend() {
				if (this.moving === false) {
					return;
				}
				this.moving = false;
				this.coverTransition = 'transform 0.3s cubic-bezier(.21,1.93,.53,.64)';
				this.coverTransform = 'translateY(0px)';
			},
			profile() {
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success: function(res) {
						uni.navigateTo({
							url: "../my/profile/profile"
						})
					},
					fail() {
						that.dengLu = true
					}
				})
			},
			collect() {
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success: function(res) {
						uni.navigateTo({
							url: "../my/collect/collect"
						})
					},
					fail() {
						that.dengLu = true
					}
				})
			},
			diZhi() {
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success: function(res) {
						uni.navigateTo({
							url: "../my/ress/ress"
						})
					},
					fail() {
						that.dengLu = true
					}
				})
			},
			QUAN(index) {
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success: function(res) {
						uni.navigateTo({
							url: "../my/quangBu/quangBu?id=" + index + ""
						})
					},
					fail() {
						that.dengLu = true
					}
				})
			},
			QU() {
				this.dengLu = false
			},
			DENGLU() {
				uni.navigateTo({
					url: "role/role"
				})
				this.dengLu = false
			},
			feng() {
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success: function(res) {
						uni.request({
							url: 'http://192.168.1.84:8081/m.api',
							data: {
								_gp: 'footprint',
								_mt: 'getAllFootprint',

							},
							header: {
								"Content-type": "application/x-www-form-urlencoded",
								'accessToken': res.data
							},
							success: (res) => {
								that.fenYe = res.data.data
							}
						});
					}
				});
			},
			deleteFeng() {
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success: function(res) {
						uni.request({
							url: 'http://192.168.1.84:8081/m.api',
							data: {
								_gp: 'footprint',
								_mt: 'deleteFootprint',
								footprintId: that.fenYe[that.num].id
							},
							header: {
								"Content-type": "application/x-www-form-urlencoded",
								'accessToken': res.data
							},
							complete() {
								that.dele = false
								that.feng()
							}
						});
					}
				});
			},
			clickFeng(e) {
				let that = this
				uni.setStorage({
					key: 'id',
					data: that.fenYe[e].spuId,
					success() {
						uni.navigateTo({
							url: "../index/input/souSuo/xiangQing/xiangQing"
						})
					}
				});
			},
			long(index) {
				this.dele = true;
				this.num = index
			}
		},
		onLoad() {
			let that = this
			uni.getStorage({
				key: 'dengLu',
				success: function(res) {
					that.dengLu = true
				},
				fail(e) {
					that.dengLu = false
				}
			});
		},
		onShow() {
			let that = this
			that.feng()
			uni.getStorage({
				key: 'dengLu',
				success: function(res) {
					that.resd = res.data
					that.dengLu = false
				},
				fail() {
					that.dengLu = true
				}
			});
			uni.getStorage({
				key: 'phone',
				success: function(res) {
					uni.request({
						url: 'http://192.168.1.84:8081/m.api',
						data: {
							_gp: 'user',
							_mt: 'login',
							phone: res.data[0],
							password: res.data[1],
							ip: '1'
						},
						header: {
							"Content-type": "application/x-www-form-urlencoded"
						},
						success: (res) => {
							if (that.resd != res.data.data) {
								that.resd = res.data.data
							}
						}
					});
				}
			});
		}
	}
</script>

<style>
	#di>view>text:last-child {
		float: right;
	}

	#di>view>text:first-child,
	#di>view>text:nth-child(2) {
		float: left;
	}

	.h-list {
		white-space: nowrap;
		padding: 30upx 30upx 0;

	}

	#di>view {
		width: 100%;
		clear: both;
		padding: 1rem 0;
		border-bottom: 1px #555555 solid;
		height: 1rem;
	}

	#di>view>text:first-child {
		margin-left: 1rem;
	}

	.order-section {
		width: 100%;
		height: 4rem;
		background-color: white;
		margin-top: 7vh;
		padding-top: 1rem;
	}

	.order-section>view {
		width: 25%;
		float: left;
	}
</style>
