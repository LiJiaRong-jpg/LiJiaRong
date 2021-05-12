<template>
	<view>

		<view style="height: 30vh;overflow: hidden;">
			<image src="../../static/user-bg.jpeg" style="width: 100%;position: absolute;z-index: -1;" mode=""></image>
			<view style="margin-left: 2rem;margin-top: 3rem;">
				<view style="float: left;">
					<image :src="resd.avatarUrl" mode=""
						style="width: 15vh;height: 15vh;position: absolute;border-radius: 50%;border: white solid 5px;">
					</image>
				</view>
				<view style="float: left;position: absolute;line-height: 15vh;margin-left: 6rem;">
					<text>{{resd.nickname}}</text></view>
			</view>
			<view style="width: 100%;position: absolute;">
				<view
					style="padding-left: 20rpx;width: 90%;margin: auto;background-color: #45433a;color: #ddb473;margin-top: 7.8rem;border-radius: 1rem 1rem 0 0;">
					<text class="iconfont" style="color: #ddb473;">&#xe623;</text>
					<text>普通会员</text>
				</view>
			</view>
		</view>



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
			<view>
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

		<view style="width: 100%;height: 100vh;background-color: rgba(0,0,0,0.6);position: fixed;z-index: 999;top: 0;"
			v-if="dengLu">
			<view style="width: 24vh;height: 10vh;background-color: white;margin: auto;margin-top: 45vh;">
				<view style="width: 100%;">您未登录，请登录</view>
				<view style="width: 50%;float: left;" @click="QU">取消</view>
				<view style="width: 50%;float: left;color: #007AFF;" @click="DENGLU" >登录</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				dengLu: true, //是否登录
				resd: null //数据
			}
		},
		methods: {
			profile(){
				uni.navigateTo({
					url:"../my/profile/profile"
				})
			},
			collect(){
				uni.navigateTo({
					url:"../my/collect/collect"
				})
			},
			diZhi(){
				uni.navigateTo({
					url:"../my/ress/ress"
				})
			},
			QUAN(index){
				uni.navigateTo({
					url:"../my/quangBu/quangBu?id="+index+""
				})
			},
			QU() {
				this.dengLu = false
			},
			DENGLU() {
				let that = this
				uni.request({
					url: 'http://192.168.1.84:8081/m.api', //仅为示例，并非真实接口地址。
					data: {
						_gp: 'user',
						_mt: 'login',
						phone: '13587878787',
						password: 'asd123456',
						ip: '1'
					},
					header: {
						"Content-type": "application/x-www-form-urlencoded"
					},
					success: (res) => {
						that.resd = res.data.data
						uni.setStorage({
							key: 'dengLu',
							data: res.data.data.accessToken
						});
					},
					complete() {
						that.dengLu = false
					}
				});
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
			let that=this
			uni.request({
				url: 'http://192.168.1.84:8081/m.api', //仅为示例，并非真实接口地址。
				data: {
					_gp: 'user',
					_mt: 'login',
					phone: '13587878787',
					password: 'asd123456',
					ip: '1'
				},
				header: {
					"Content-type": "application/x-www-form-urlencoded"
				},
				success: (res) => {
					that.resd = res.data.data
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
		margin-top: 2rem;
		padding-top: 1rem;
	}

	.order-section>view {
		width: 25%;
		float: left;
	}
</style>
