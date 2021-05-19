<template>
	<view class="about">
		<view><text>联系人</text></view>
		<view><input type="text" v-model="name" placeholder="收货人姓名" /></view>
		<view><text>手机号</text></view>
		<view><input type="text" v-model="phone" placeholder="收货人电话号码" /></view>
		<view><text>城市</text></view>
		<view>
			<view class="uni-list">
				<picker mode="region" :range="index" @change="bindPickerChange">
					<view>{{index[0]}}
						{{index[1]}}
						{{index[2]}}
					</view>
				</picker>
			</view>
		</view>
		<view> <text>详细</text></view>
		<view> <input type="text" v-model="diZhi" placeholder="街道、楼号、门牌" /></view>
		<view> <text>设置为默认</text></view>
		<view>
			<switch :checked="autoplay" @change="changeAutoplay" />
		</view>
		<view style="width: 80%;background-color: #fc4369;margin-left: 5%;border-radius: 1rem;
			padding:0 1rem ;line-height: 2rem;position: fixed;bottom: 10px;" @click="addRess" v-if="add==true">
			<text style="display: table;margin: auto;">提交</text>
		</view>
		<view style="width: 80%;background-color: #fc4369;margin-left: 5%;border-radius: 1rem;
			padding:0 1rem ;line-height: 2rem;position: fixed;bottom: 10px;" @click="splitRess" v-if="add==false">
			<text style="display: table;margin: auto;">提交</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				add: true,
				autoplay: false,
				num: 0,
				index: [], //省市区
				name: '', //名字
				phone: '', //电话
				diZhi: '', //详细地址
				gp: '',
				mt: '',
				id: '' //要变更的地址ID
			}
		},
		computed: {

		},
		methods: {
			changeAutoplay(e) {
				this.autoplay = !this.autoplay
				if (this.autoplay == true) {
					this.num = 1
				} else {
					this.num = 0
				}
			},
			bindPickerChange: function(e) {
				this.index = e.target.value
			},
			getDate(type) {
				const date = new Date();
				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();
				if (type === 'start') {
					year = year - 60;
				} else if (type === 'end') {
					year = year + 2;
				}
				month = month > 9 ? month : '0' + month;;
				day = day > 9 ? day : '0' + day;
				return `${year}-${month}-${day}`;
			},
			addRess() {
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success: function(res) {
						uni.request({
							url: "http://192.168.1.84:8081/m.api", //仅为示例，并非真实接口地址。
							data: {
								consignee: that.name,
								phone: that.phone,
								province: that.index[0],
								city: that.index[1],
								county: that.index[2],
								address: that.diZhi,
								defaultAddress: that.num,
								def: that.autoplay,
								_gp: that.gp,
								_mt: that.mt
							},
							method:"POST",
							header: {
								"Content-type": "application/x-www-form-urlencoded",
								"accesstoken": res.data
							},
							success: (resd) => {
								uni.navigateTo({
									url: "../ress"
								})
							}
						});
					}
				});
			},
			splitRess() {
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success: function(res) {
						uni.request({
							url: "http://192.168.1.84:8081/m.api", //仅为示例，并非真实接口地址。
							data: {
								consignee: that.name,
								phone: that.phone,
								province: that.index[0],
								city: that.index[1],
								county: that.index[2],
								addressId: that.id,
								address: that.diZhi,
								defaultAddress: that.num,
								def: that.autoplay,
								_gp: that.gp,
								_mt: that.mt,
								id: that.id
							},
							header: {
								"Content-type": "application/x-www-form-urlencoded",
								"accesstoken": res.data
							},
							success: (resd) => {
								uni.navigateBack({
									delta: 1
								});
							}
						});
					}
				});
			}
		},
		onLoad(e) {
			this.gp = e.gp
			this.mt = e.mt
			if (e.consignee) {
				this.name = e.consignee
				this.phone = e.phone
				this.diZhi = e.address
				this.id = e.id
				this.index.push(e.province)
				this.index.push(e.city)
				this.index.push(e.county)
				this.add = false
			} else {
				this.index = ["北京市", "北京市", "东城区"]
			}
		}
	}
</script>

<style>
	.about>view:nth-child(odd) {
		float: left;
		margin: 0 10px;
		width: 25%;
		margin-bottom: 1rem;
	}

	.about>view:nth-child(even) {
		float: left;
		width: 65%;
		margin-bottom: 1rem;
	}
</style>
