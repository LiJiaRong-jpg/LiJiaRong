<template>
	<view>
		<view style="width: 30%;float: left;height:100vh ;overflow-y: auto;">
			<view v-for="(key , index) in DATA" :class="clas==index?'num':''" style="padding: 1rem 0;"
				:data-id="index" @click="nav">
				<text style="text-align: center;display: block;"
					:class="clas==index?'indexText':''">{{key.title}}</text>
			</view>
		</view>

		<view
			style="width: 70%;float: left;padding-left: 10rpx;background-color: #f8f8f8;box-sizing: border-box;height:100vh ;overflow-y: auto;">
			<view v-for="(key , index) in DATA" v-if="clas==index">
				<view v-for="(Key,Index) in key.childrenList" style="width: 100%;">
					
					<view style="background-color: #f8f8f8;">
						<text>{{Key.title}}</text>
					</view>


					<view style="width: 100%;background-color: white;display: inline-block;">
						<view v-for="(KEy,INdex) in Key.childrenList" style="width: 33%;float: left;" @click="xQ(KEy)">
							<view>
								<image :src="KEy.picUrl" mode="" style="width:16vh;height: 16vh"></image>
							</view>
							<view>
								<text>{{KEy.title}}</text>
							</view>
						</view>
					<view style="width: 100%;height: 2rpx;clear: both;background-color: white;"></view>
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
				DATA: [],
				clas: 0
			}
		},
		methods: {
			nav(e) {
				this.clas = e.currentTarget.dataset.id
			},
			xQ(key){
				console.log(key)
				uni.setStorage({
				    key: 'titleA',
				    data: key.id,
					success() {
						uni.navigateTo({
							url:"../index/input/souSuo/souSuo"
						})
					}
				});
			}
		},
		onLoad() {
			let that = this
			uni.request({
				url: 'http://192.168.1.54:8081/m.api',
				data: {
					_gp: 'category',
					_mt: 'categoryList'
				},
				method: "GET",
				header: {
					"Content-type": "application/x-www-form-urlencoded"
				},
				success: (res) => {
					that.DATA = res.data.data
				}
			});
		}
	}
</script>

<style>
	.num {
		background-color: #f8f8f8;
	}

	.indexText {
		border-left: 10rpx solid #fe7b92;
		color: #fe7b92;
	}
</style>
