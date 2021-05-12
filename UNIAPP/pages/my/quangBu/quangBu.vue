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
				<swiper-item v-for="(key,index) in contentList" :key="index" style="height: 100%;">
					<view class="content" style="overflow: auto;">


						{{key}}
						没数据做🔨
						<view v-if="index==0"
							style="width: 100%;height: 2rem;position: fixed;bottom: 0;border-top: #000000 solid 2px;">
							{{msg}}
						</view>
					</view>
				</swiper-item>
			</swiper>
		</view>


	</view>
</template>

<script>
	export default {
		data() {
			return {
				tabIndex: 0,
				/* 选中标签栏的序列,默认显示第一个 */
				contentList: [
					"全部",
					"代付款",
					"待出库",
					"待收货",
					'待评价',
					'退款中',
				],
				tabBars: [{
						name: '全部',
						id: 'guanzhu'
					},
					{
						name: '代付款',
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
				data: [], //数据
				msg: "未登录"
			}
		},
		methods: {
			//切换选项卡
			toggleTab(index) {
				this.tabIndex = index;
			},
			//滑动切换swiper
			tabChange(e) {
				this.tabIndex = e.detail.current;
			}
		},
		onLoad(e) {
			this.tabIndex = e.id //加载时定位到哪个选项

			let that = this
			uni.getStorage({
				key: 'dengLu',
				success: function(res) {
					uni.request({
						url: 'http://192.168.1.84:8081/m.api', //仅为示例，并非真实接口地址。
						data: {
							_gp: 'order',
							_mt: 'getOrderPage',
							pageNo: 1,
							status: 0
						},
						header: {
							"Content-type": "application/x-www-form-urlencoded",
							"accesstoken": res.data
						},
						success: (resd) => {
							that.msg = resd.data.data.msg
						}
					});
				}
			});
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
