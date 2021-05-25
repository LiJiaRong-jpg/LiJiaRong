<!-- 横向选项卡（水平滚动导航栏） -->
<template>
	<view>
		<!-- 顶部导航栏 -->
		<view class="horizonal-tab">
			<scroll-view scroll-x="true" scroll-with-animation class="scroll-tab">
				<block v-for="(item,index) in tabBars" :key="index">
					<view class="scroll-tab-item" :class="{'active': tabIndex==index}" @tap="toggleTab(index)">
						{{item.name}}
						<view class="scroll-tab-line"></view>
					</view>
				</block>
			</scroll-view>
		</view>
		<!-- 内容区 -->
		<view class="content" style="height: 93vh;">
			<!-- 滑块视图 -->
			<swiper :current="tabIndex" @change="tabChange" style="height: 100%;">
				<!-- current:当前所在滑块的index -->
				<swiper-item key="0" style="height: 100%;">
					<view class="content" style="overflow-y: auto;height: 100vh;">
						<view v-for="(key,index) in contentList">
							<view>
								<text style="float:left;">{{key.skuList[0].gmtCreate}}</text>
								<text style="float:right;color: #febecc;">{{statusMap[key.status]}}</text>
							</view>
							<view style="width: 100%;clear: both;">
								<view style="width: 100%;">
									<image :src="key.skuList[0].img" mode=""
										style="width: 5rem;height: 5rem;float: left;"></image>
									<view
										style="float: left;overflow:hidden;white-space:nowrap;text-overflow:ellipsis;width: 70%">
										<text>{{key.skuList[0].spuTitle}}\n</text>
										<text>{{key.skuList[0].title}}×{{key.skuList[0].num}}\n</text>
										<text>￥{{key.skuList[0].price/100}}\n</text>
									</view>
								</view>
								<view style="width: 100%;height: 2rem;clear: both;border-bottom: #999999 solid 1px;">
									<text
										style="float: right;">共{{key.skuList[0].num}}件商品,实付款￥{{key.skuList[0].price/100*key.skuList[0].num}}</text>
								</view>
								<view style="width: 100%;height: 5rem;clear: both;" v-if="statusMap[key.status]=='待出库'">
									<view
										style="width: 4rem;line-height: 2rem;display: block;text-align: center;float: right;border: #999999 1px solid;margin: 1rem 0;border-radius: 2rem;margin-right: 1rem;padding: 0 1rem;">
										申请退款
									</view>
								</view>
								<view style="width: 100%;height: 5rem;clear: both;" v-if="statusMap[key.status]=='未付款'">
									<view style="width: 4rem;line-height: 2rem;display: block;text-align: center;float: right;border: #999999 1px solid;
									margin: 1rem 0;border-radius: 2rem;margin-right: 1rem;padding: 0 1rem;" @click="zhiFu(index)">
										立即支付
									</view>
									<view style="width: 4rem;line-height: 2rem;display: block;text-align: center;float: right;border: #FC4369 1px solid;
									margin: 1rem 0;border-radius: 2rem;margin-right: 1rem;padding: 0 1rem;color: #FC4369;" @click="quxiao(index)">
										取消订单
									</view>
								</view>
							</view>
							<view style="width: 100%;height: 5px;background-color: #C0C0C0;clear: both;"></view>
						</view>
						<view style="width: 100%;height: 5rem;"></view>
						<view style="width: 100%;height: 2rem;position: fixed;bottom: 0;border-top: #000000 solid 2px;
							background-color: white;">
							{{msg}}
						</view>
					</view>
				</swiper-item>
				<swiper-item key="1" style="height: 100%;">
					<view class="content" style="overflow-y: auto;height: 100vh;">
						<view v-for="(key,index) in contentList" v-if="statusMap[key.status]=='未付款'">
							<view>
								<text style="float:left;">{{key.skuList[0].gmtCreate}}</text>
								<text style="float:right;color: #febecc;">{{statusMap[key.status]}}</text>
							</view>
							<view style="width: 100%;clear: both;">
								<view style="width: 100%;">
									<image :src="key.skuList[0].img" mode=""
										style="width: 5rem;height: 5rem;float: left;"></image>
									<view
										style="float: left;overflow:hidden;white-space:nowrap;text-overflow:ellipsis;width: 70%">
										<text>{{key.skuList[0].spuTitle}}\n</text>
										<text>{{key.skuList[0].title}}×{{key.skuList[0].num}}\n</text>
										<text>￥{{key.skuList[0].price/100}}\n</text>
									</view>
								</view>
								<view style="width: 100%;height: 2rem;clear: both;border-bottom: #999999 solid 1px;">
									<text
										style="float: right;">共{{key.skuList[0].num}}件商品,实付款￥{{key.skuList[0].price/100*key.skuList[0].num}}</text>
								</view>
								<view style="width: 100%;height: 5rem;clear: both;">
									<view  @click="zhiFu(index)"
										style="width: 4rem;line-height: 2rem;display: block;text-align: center;float: right;border: #FC4369 1px solid;margin: 1rem 0;border-radius: 2rem;margin-right: 1rem;padding: 0 1rem;color: #FC4369;">
										立即支付</view>
									<view @click="quxiao(index)"
										style="width: 4rem;line-height: 2rem;display: block;text-align: center;float: right;border: #999999 1px solid;margin: 1rem 0;border-radius: 2rem;margin-right: 1rem;padding: 0 1rem;">
										取消订单</view>
								</view>
							</view>
							<view style="width: 100%;height: 5px;background-color: #C0C0C0;clear: both;"></view>
						</view>
						<view style="width: 100%;height: 5rem;"></view>
					</view>
				</swiper-item>

				<swiper-item key="2" style="height: 100%;">
					<view class="content" style="overflow-y: auto;height: 100vh;">
						<view v-for="(key,index) in contentList" v-if="statusMap[key.status]=='待出库'">
							<view>
								<text style="float:left;">{{key.skuList[0].gmtCreate}}</text>
								<text style="float:right;color: #febecc;">{{statusMap[key.status]}}</text>
							</view>
							<view style="width: 100%;clear: both;">
								<view style="width: 100%;">
									<image :src="key.skuList[0].img" mode=""
										style="width: 5rem;height: 5rem;float: left;"></image>
									<view
										style="float: left;overflow:hidden;white-space:nowrap;text-overflow:ellipsis;width: 70%">
										<text>{{key.skuList[0].spuTitle}}\n</text>
										<text>{{key.skuList[0].title}}×{{key.skuList[0].num}}\n</text>
										<text>￥{{key.skuList[0].price/100}}\n</text>
									</view>
								</view>
								<view style="width: 100%;height: 2rem;clear: both;border-bottom: #999999 solid 1px;">
									<text
										style="float: right;">共{{key.skuList[0].num}}件商品,实付款￥{{key.skuList[0].price/100*key.skuList[0].num}}</text>
								</view>
								<view style="width: 100%;height: 5rem;clear: both;">
									<view
										style="width: 4rem;line-height: 2rem;display: block;text-align: center;float: right;border: #999999 1px solid;margin: 1rem 0;border-radius: 2rem;margin-right: 1rem;padding: 0 1rem;">
										申请退款</view>
								</view>
							</view>
							<view style="width: 100%;height: 5px;background-color: #C0C0C0;clear: both;"></view>
						</view>
						<view style="width: 100%;height: 5rem;"></view>
					</view>
				</swiper-item>

				<swiper-item key="3" style="height: 100%;">
					4
				</swiper-item>

				<swiper-item key="4" style="height: 100%;">
					5
				</swiper-item>

				<swiper-item key="5" style="height: 100%;">
					6
				</swiper-item>

			</swiper>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				statusMap: {
					10: '未付款',
					12: '正在拼团',
					20: '待出库',
					30: '待收货',
					40: '待评价',
					50: '已完成',
					60: '退款中',
					70: '已退款',
					80: '已取消',
					90: '已取消(系统)'
				},
				tabIndex: 1,
				contentList: [],
				tabBars: [{
						name: '全部',
						id: 'guanzhu'
					},
					{
						name: '待付款',
						id: 'tuijian'
					},
					{
						name: '待出库',
						id: 'redian'
					},
					{
						name: '待收货',
						id: 'tiyu'
					},
					{
						name: '待评价',
						id: 'caijing'
					},
					{
						name: '退款中',
						id: 'yule'
					}
				],
				data: [], //总数据
				msg: "未登录"
			}
		},
		methods: {
			zhiFu(index) {
				let that = this
				uni.setStorage({
					key: 'orderNo',
					data: [that.contentList[index].orderNo, that.contentList[index].actualPrice],
					success: function(res) {
						uni.navigateTo({
							url: '../../index/input/souSuo/dingDang/dingDang'
						})
					},
				})
			},
			quxiao(index) {
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success: function(res) {
						uni.request({
							url: 'http://192.168.1.54:8081/m.api', //仅为示例，并非真实接口地址。
							data: {
								_gp: 'order',
								_mt: 'cancel',
								orderNo: that.contentList[index].orderNo
							},
							header: {
								"Content-type": "application/x-www-form-urlencoded",
								"accesstoken": res.data
							},
							success: (res) => {
								that.load()
								uni.showToast({
									title: res.data.errmsg,
									duration: 2000
								});
							}
						});
					}
				});
			},
			//切换选项卡
			toggleTab(index) {
				this.tabIndex = index;
			},
			//滑动切换swiper
			tabChange(e) {
				this.tabIndex = e.detail.current;
			},
			time(token) {
				let that = this
				for (let i in this.contentList) {
					let time = this.contentList[i].skuList[0].gmtCreate //获取时间戳(这部分是生成时间)
					time = new Date(time)
					this.contentList[i].skuList[0].gmtCreate = (time.getFullYear()) + "-" + (time.getMonth() + 1) + "-" + (
						time.getDate()) + " " + (time.getHours()) + ":" + (time.getMinutes())
				}
			},
			load() {
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success: function(res) {
						uni.request({
							url: 'http://192.168.1.54:8081/m.api', //仅为示例，并非真实接口地址。
							data: {
								_gp: 'order',
								_mt: 'getOrderPage',
								pageNo: 1,
								pageSize: 10
							},
							header: {
								"Content-type": "application/x-www-form-urlencoded",
								"accesstoken": res.data
							},
							success: (resd) => {
								that.msg = resd.data.data.msg
								that.contentList = resd.data.data.items
								that.time(res.data)
							}
						});
					}
				});
			}
		},
		onShow() {
			let that = this
			uni.getStorage({
				key: 'index',
				success: function(res) {
					that.load()
					that.tabIndex=res.data
				}
			})
		}
	}
</script>

<style>
	.horizonal-tab {
		height: 7vh;
	}

	.horizonal-tab .active {
		color: red;
	}

	.scroll-tab {
		white-space: nowrap;
		/* 必要，导航栏才能横向*/
		border-bottom: 1rpx solid #eee;
		text-align: center;
	}

	.scroll-tab-item {
		display: inline-block;
		/* 必要，导航栏才能横向*/
		margin: 20rpx 30rpx 0 30rpx;
	}

	.active .scroll-tab-line {
		border-bottom: 5rpx solid red;
		border-top: 5rpx solid red;
		border-radius: 20rpx;
		width: 70rpx;

	}
</style>
