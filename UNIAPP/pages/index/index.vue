<template>

	<view class="content">
		<view class="input" :style="back">
			<text class="iconfont">&#xe60e;</text>
			<input type="text" value="" placeholder="请输入宝贝名称" />
			<text class="iconfont">&#xe60c;</text>
		</view>

		<view class="uni-padding-wrap">
			<view class="page-section swiper">
				<view class="page-section-spacing">
					<swiper class="swiper" indicator-dots="true" autoplay="true" interval="3000" duration="400">
						<swiper-item v-for="(key,index) in lunBo">
							<view style="background-color:{{key.color}}"></view>
							<image :src="key.imgUrl" mode="" style="width: 100%;" />
						</swiper-item>
					</swiper>
				</view>
			</view>
		</view>


		<view class="nav" style="margin: 1rem 0;">
			<view class="" v-for="(key , index) in nav">
				<image :src="key.imgUrl" mode=""></image>
				<view><text>{{key.title}}</text></view>
			</view>
		</view>


		<view>
			<view style="width: 100%;height: 5rem;">
				<image src="../../static/h1.png"
					style="float: left;width: 2rem;height: 2rem;margin-left: 2rem;margin-right: 1rem;"></image>
				<view style="float: left;">
					<text>橱窗推荐</text>
					<text style="color: #999999;">\nshop window</text>
				</view>
			</view>

		</view>


		<view class="c" style="width: 100%;margin-bottom: 1rem;height: 20rem;">
			<view class="" v-for="(key , index) in chuChuang" style="width: 46%;margin-left: 2%;float: left;">
				<image :src="key.spuImg" mode="" style="width: 100%;"></image>
				<view style="overflow:hidden;white-space:nowrap;text-overflow:ellipsis;width: 80%;">
					<text>{{key.spuTitle}}</text>
				</view>
				<text style="color: red;">￥{{key.spuPrice/100}}</text>
				<text style="text-decoration: line-through;color: #808080;">￥{{key.spuOriginalPrice}}</text>
				<text>累计销售：{{key.spuSales}}件</text>
			</view>
		</view>


		<view style="width: 100%;height: 4rem;margin-top: 1rem;">
			<image src="../../static/h1.png"
				style="float: left;width: 2rem;height: 2rem;margin-left: 2rem;margin-right: 1rem;"></image>
			<view style="float: left;">
				<text>分类精选</text>
				<text style="color: #999999;">\nCompetitive Products For You</text>
			</view>
		</view>



		<view class="" v-for="(key , index) in nava" style="margin-bottom: -8.3rem;">
			<image :src="key.imgUrl" style="width: 100%;"></image>
			<view
				style="position: relative;top: -8rem;width: 95%;margin-left: 5%;background-color: white;height: 8rem;">
				<view>
					<view>
						<image :src="key.data[0].img" style="width: 5rem;height: 5rem;"></image>
					</view>
					<view style="overflow:hidden;white-space:nowrap;text-overflow:ellipsis;width: 5rem;">
						<text>{{key.data[0].title}}</text>
					</view>
				</view>
			</view>
		</view>


		<view style="width: 100%;height: 5rem;margin-top: 1rem;">
			<image src="../../static/h1.png"
				style="float: left;width: 2rem;height: 2rem;margin-left: 2rem;margin-right: 1rem;"></image>
			<view style="float: left;">
				<text>热销推荐</text>
				<text style="color: #999999;">\nHot Sale</text>
			</view>
		</view>


		<view class="c" style="width: 100%;">
			<view class="" v-for="(key , index) in shopping" style="width: 46%;margin-left: 2%;float: left;">
				<image :src="key.img" mode="" style="width: 100%;"></image>
				<view style="overflow:hidden;white-space:nowrap;text-overflow:ellipsis;width: 80%;">
					<text>{{key.title}}</text>
				</view>
				<text style="color: red;">￥{{key.price/100}}</text>
			</view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				back: "background-color:rgba(255,255,255,0)",
				lunBo: [], //轮播图的图片
				nav: [], //轮播图下的导航条
				nava: [], //精选的背景图
				shopping: [], //商品栏
				chuChuang: [] //橱窗推荐

			}
		},
		onLoad() {
			let that = this
			uni.request({
				url: 'http://192.168.1.84:8081/m.api',
				data: {
					_gp: 'integral',
					_mt: 'getIndexData'
				},
				method: "POST",
				header: {
					"Content-type": "application/x-www-form-urlencoded"
				},
				success: (res) => {
					that.lunBo = res.data.data.advertisement.t1
					that.nav = res.data.data.advertisement.t4
					that.nava = res.data.data.advertisement.t2
					that.shopping = res.data.data.salesTop
					that.chuChuang.push(res.data.data.windowRecommend[0])
					that.chuChuang.push(res.data.data.windowRecommend[1])
				}
			});
		},
		methods: {

		},
		onPageScroll(res) { //页面滚动事件
			this.top = res.scrollTop;
			let a = 0
			if (res.scrollTop / 300 > 1) {
				this.back = "background-color:rgba(255,255,255,1)"
			} else {
				this.back = "background-color:rgba(255,255,255," + res.scrollTop / 300 + ")"
			}
		},
	}
</script>

<style>
	.input {
		position: fixed;
		top: 0;
		width: 100%;
		z-index: 9999;
		padding-bottom: 1rem;
		padding-top: 4rem;
	}

	.input>input,
	.input>text:first-child {
		float: left;
		margin-left: 0.5rem;
	}

	.input>input {
		width: 70%;
		border: #C0C0C0 2rpx solid;
		border-radius: 3rem;
		padding-left: 0.5rem;
	}

	.uni-padding-wrap swiper-item>view {
		width: 100%;
		height: 6.5rem;
	}

	.input>text:last-child {
		float: right;
		margin-right: 0.5rem;
	}

	swiper {
		height: calc(93vw*5/5)
	}

	.nav {
		width: 100%;
		height: 5rem;
	}

	.nav>view {
		width: 18%;
		height: 100%;
		float: left;
		margin-left: 1.5%;
	}

	.nav image {
		width: 100%;
		height: 70%;
	}
</style>
