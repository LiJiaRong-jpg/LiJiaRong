<template>
	<view>
		<view class="inputA">
			<view @click="souSuo=true">
				<input type="text" v-model="input" placeholder="默认关键字" />
				<view v-if="souSuo">
					<view @click.stop="clear">清除</view>
					<view @click="sou">搜索</view>
				</view>
			</view>
		</view>
		<view v-if="you" style="width: 95%;margin-left: 5%;margin-top: 2rem;">
			<text style="margin-bottom: 1rem;">历史记录</text>
			<view>
				<view v-for="key in liShi" @click="YOU(key)"
					style="border-radius: 2rem;background-color: #f2f2f2;padding: 5px 10px;display: initial;margin: 0.5rem;float: left;">
					{{key}}
				</view>
			</view>
			<text @click='clearLiShi' style="float: right;clear: both;">清除历史记录</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				souSuo: false,
				input: "",
				liShi: "",
				you: true
			}
		},
		methods: {
			clear() {
				this.souSuo = false
				this.input = ""
			},
			sou() {
				if (this.input.trim() != '') {
					this.kaiShi()
				}
			},
			YOU(e){
				this.input=e
				this.kaiShi()
			},
			kaiShi() {
				let that = this
				let arr=[]
				arr.push(that.input.trim())
				arr.push(10)
				uni.setStorage({
					key: 'title',
					data: arr
				});
				uni.navigateTo({
					url: "./souSuo/souSuo"
				})
				uni.getStorage({
					key: 'liShi',
					success: function(res) {
						let arr = JSON.parse(res.data)
						arr.push(that.input.trim())
						arr = JSON.stringify(arr)
						uni.setStorage({
							key: 'liShi',
							data: arr
						});
					},
					fail(res) {
						let arr = []
						arr.push(that.input.trim())
						arr = JSON.stringify(arr)
						uni.setStorage({
							key: 'liShi',
							data: arr
						});
					}
				});
			},
			clearLiShi() {
				let that = this
				uni.removeStorage({
					key: 'liShi',
					success: function(res) {
						that.liShi = ''
						that.you = false
					}
				});
			}
		},
		onShow() {
			let that = this
			uni.getStorage({
				key: 'liShi',
				success: function(res) {
					that.liShi = JSON.parse(res.data)
				},
				fail(res) {
					that.you = false
				}
			});
		}
	}
</script>

<style>
	.inputA {
		width: 100%;
		height: 2rem;
		padding: 5px 0;
		background-color: #f2f2f2;
	}

	.inputA>view {
		width: 80%;
		margin: auto;
		border-radius: 2rem;
		padding-left: 1rem;
		background-color: white;
		height: 2rem;
		overflow: hidden;
	}

	.inputA>view>input {
		line-height: 2rem;
		margin-top: 5px;
		float: left;
	}

	.inputA>view>view {
		float: right;
	}

	.inputA>view>view>view {
		float: left;
	}

	.inputA>view>view>view:last-child {
		padding: 0 19px;
		background: linear-gradient(to right, #ff9801, #ff570a);
		line-height: 2rem;
		color: #fff;
		transition: all 0.3s;
	}

	.inputA>view>view>view:first-child {
		line-height: 2rem;
	}
</style>
