<template>
	<view>
		<view class="nav">
			<view @click="sopping(0)" :class="cla==0?'col':''"><text>销量优先</text></view>
			<view @click="sopping(1)" :class="cla==1?'col':''"><text>价格</text></view>
		</view>
		<view style="width: 100%;height: 5px;background-color: #999999;	margin-bottom: 1rem;"></view>
		<view style="width: 100%;">
			<view v-for="(key,index) in shuJu" class="shopping">
				<view>
					<image :src="key.img" mode="" style="width: 100%;"></image>
				</view>
				<view>
					<text>{{key.title}}</text>
				</view>
				<view>
					<text>￥{{key.price/100}}</text>
					<text>已售{{key.sales}}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				cla: 1,
				orderBy: 'sales',
				isAsc: true,
				title: "",
				shuJu: []
			}
		},
		methods: {
			sopping(index) {
				if (index == 0) {
					this.cla = 0
				} else {
					this.cla = 0
					this.cla = 1
					this.isAsc = !this.isAsc
				}
			}
		},
		onShow() {
			let that = this
			uni.getStorage({
				key: 'title',
				success: function(res) {
					that.title = res.data
					that.cla = 0
					console.log(that.title)
				}
			});
		},
		watch: {
			cla() {
				let that = this
				uni.request({
					url: 'http://192.168.1.84:8081/m.api', //仅为示例，并非真实接口地址。
					data: {
						_gp: 'goods',
						_mt: 'getGoodsPage',
						pageNo: 1,
						pageSize: 10,
						orderBy: that.orderBy,
						isAsc: that.isAsc,
						title: that.title
					},
					method: "GET",
					header: {
						"Content-type": "application/x-www-form-urlencoded",

					},
					success: (res) => {
						that.shuJu = res.data.data.items
						console.log(that.shuJu)
					}
				});
			}
		}
	}
</script>

<style>
	.nav {
		display: flex;
	}

	.nav>view {
		width: 50%;
		float: left;
		text-align: center;
	}

	.col {
		color: #fc4369;

	}

	.col>text {
		border-bottom: 5px solid #FC4369;
	}

	.nav text {
		line-height: 3rem;
		display: inline-block;
		width: 40%;
	}

	.shopping {
		width: 50%;
		float: left;
	}

	.shopping>view {
		width: 100%;
		overflow:hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
	}

	.shopping>view:last-child>text:first-child {
		float: left;
		color: #FC4369;
	}

	.shopping>view:last-child>text:last-child {
		float: right;
	}
</style>
