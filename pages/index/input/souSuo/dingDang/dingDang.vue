<template>
	<view>
		<view style="padding-left: 10px;width: 100%;height: 3rem;clear: both;border-bottom: #C0C0C0 solid 1px;">
			<text>{{weiZhi.consignee}} {{weiZhi.phone}}\n</text>
			<text>{{weiZhi.province}} {{weiZhi.city}} {{weiZhi.county}} {{weiZhi.address}}</text>
			<text class="iconfont" style="float: right;margin-right: 10rpx;">&#xe611;</text>
		</view>


		<view style="clear: both;border-bottom: #808080 solid 1px;display: flex;">
			<view class="img">
				<image :src="ding.skuList[0].spuImg" mode=""></image>
			</view>
			<view class="buttom">
				<text>{{ding.skuList[0].title}}\n</text>
				<text>{{ding.skuList[0].skuTitle}}\n</text>
				<text style="text-decoration: line-through;">￥{{ding.skuList[0].originalPrice/100}}</text>
				<text>￥{{ding.skuList[0].price/100}}*{{ding.skuList[0].num}}</text>
			</view>
		</view>
		<view style="clear: both;border-bottom: #808080 solid 1px;display: flow-root;">
			<text style="float: left;width: 20%;">商品金额</text>
			<text style="float: right;margin-right: 5px;">¥{{ding.totalOriginalPrice/100}}</text>
		</view>
		<view style="clear: both;border-bottom: #808080 solid 1px;display: flow-root;">
			<text style="float: left;width: 20%;">折扣金额</text>
			<text
				style="float: right;margin-right: 5px;color: #FC4369;">-¥{{(ding.totalOriginalPrice-ding.totalPrice)/100}}</text>
		</view>
		<view style="clear: both;border-bottom: #808080 solid 1px;display: flow-root;">
			<text style="float: left;width: 20%;">运费</text>
			<text style="float: right;margin-right: 5px;">免运费</text>
		</view>
		<view style="clear: both;border-bottom: #808080 solid 1px;display: flow-root;">
			<text style="float: left;width: 20%;">备注</text>
			<input style="float: left;" type="text" v-model="input" />
		</view>
		<view style="clear: both;position: fixed;bottom: 0;width: 100%;border-top: #808080 solid 1px;">
			<text style="float: left;line-height: 3rem;margin-left: 10px;">实付款:</text>
			<text style="float: left;color: #FC4369;line-height: 3rem;">¥{{ding.totalPrice/100}}</text>
			<view @click="buy"
				style="float: right;line-height: 3rem;width: 30%;background-color: #FC4369;color: white;text-align: center;">
				提交订单</view>
		</view>
		<!-- 支付页面，由于层次限制所以用定位来覆盖提交订单页面 -->
		<view v-if="zhiFu==true"
			style="width: 100%;height: 100vh;overflow-y: auto;top: 0;position: fixed;background-color: white;z-index: 999;">
			<view style="text-align: center;">
				<text>支付金额</text>
				<text>\n¥{{ding.totalPrice/100}}</text>
			</view>


			<view class="uni-list" style="width: 80%;margin: auto;">
				<radio-group @change="radioChange">
					<label class="uni-list-cell uni-list-cell-pd" v-for="(item, index) in items" :key="item.value"
						style="clear: both;width: 100%;display: flow-root;margin-top: 1rem;">
						<view v-if="index==0" style="float: left;color: #68cc2b;" class="iconfont" >&#xe627;</view>
						<view v-if="index==1" style="float: left;color: #fcad2a;" class="iconfont" >&#xe629;</view>
						<view v-if="index==2" style="float: left;color: #0096d8;" class="iconfont" >&#xe628;</view>
						<view style="float: left;">{{item.name}}</view>
						<view style="float: right;">
							<radio :value="item.value" :checked="index === current" />
						</view>
					</label>
				</radio-group>
			</view>
			<view style="width: 100%;height: 20rem;"></view>
			<view @click="buyA" style="padding:10px 0;width: 80%;margin: auto;color: white;background-color: #FC4369;text-align: center;border-radius: 6px;">确认支付</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				zhiFu: false,
				qwer: null,
				orderNo: null,
				weiZhi: null,
				ding: null, //订单数据
				items: [{
					value: 'USA',
					name: '微信支付'
				}, {
					value: 'CHN',
					name: '支付宝',
					checked: 'true'
				}, {
					value: 'BRA',
					name: '线下支付(到付)'
				}],
				current: 0
			}
		},
		methods: {
			radioChange: function(evt) {
				for (let i = 0; i < this.items.length; i++) {
					if (this.items[i].value === evt.target.value) {
						this.current = i;
						break;
					}
				}
			},
			buy() {
				let that = this
				this.zhiFu = true
				let a
				uni.getStorage({
					key: 'dengLu',
					success(e) {
						a = e.data
					}
				})
				uni.getStorage({
					key: 'toke',
					success: function(res) {
						res.data
						uni.request({
							url: 'http://192.168.1.84:8081/m.api', //仅为示例，并非真实接口地址。
							data: {
								orderRequest: JSON.stringify(res.data),
								_gp: 'order',
								_mt: 'takeOrder',
								channel: 'android'
							},
							method: "POST",
							header: {
								"Content-type": "application/x-www-form-urlencoded",
								"accesstoken": a
							},
							success: (res) => {
								that.orderNo = res.data.data
							}
						});
					}
				});
			},
			buyA() {
				if (this.current == 0) {
				} else if (this.current == 1) {
				} else {
					let that = this
					uni.getStorage({
						key: 'dengLu',
						success: function(res) {
							uni.request({
								url: "http://192.168.1.84:8081/m.api", //仅为示例，并非真实接口地址。
								data: {
									_gp: 'order',
									_mt: 'offlinePrepay',
									orderNo: that.orderNo
								},
								method: "POST",
								header: {
									"Content-type": "application/x-www-form-urlencoded",
									"accesstoken": res.data
								},
								success: (resd) => {
								}
							});
						}
					});
				}

				uni.navigateBack({
					delta: 1
				});
			}
		},
		onShow() {
			let that = this
			uni.getStorage({
				key: 'dengLu',
				success: function(res) {
					uni.request({
						url: "http://192.168.1.84:8081/m.api", //仅为示例，并非真实接口地址。
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
							that.weiZhi = resd.data.data
						}
					});
				}
			});
			uni.getStorage({
				key: 'toke',
				success: function(res) {
					that.ding = res.data
				},
			})
		}
	}
</script>

<style>
	.img {
		float: left;
	}

	.img>image {
		width: 30vh;
		height: 30vh;

	}

	.buttom {
		width: 40%;
		overflow: hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
		float: left;
	}
</style>
