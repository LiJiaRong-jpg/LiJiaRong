<template>
	<view>

		<view class="about">
			<view @click="one=true"><text>修改昵称</text><text class="iconfont">&#xe61f;</text></view>
			<view @click="two=true"><text>修改性别</text><text class="iconfont">&#xe61f;</text></view>
		</view>

		<view v-if="one" class="gai">
			<view>
				<text>编辑昵称</text>
				<input type="text" v-model="Name" placeholder="修改后的名称" />
				<view class="gaI">
					<text @click="one=false">取消</text>
					<text @click="name" style="color: #007AFF;">确定</text>
				</view>
			</view>
		</view>


		<view v-if="two" class="gai">
			<view>
				<text>编辑性别</text>
				<view class="uni-list">
					<radio-group @change="radioChange">
						<label class="uni-list-cell uni-list-cell-pd" v-for="(item, index) in items" :key="item.value"
							style="float: left;">
							<view>
								<radio :value="item.value" :checked="index === current" />
							</view>
							<view>{{item.name}}</view>
						</label>
					</radio-group>
				</view>
				<view style="clear: both;" class="gaI">
					<text @click="two=false">取消</text>
					<text @click="sex" style="color: #007AFF;">确定</text>
				</view>
			</view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				one: false,
				two: false,
				items: [{
						value: '0',
						name: '保密',
						checked: 'true'
					},
					{
						value: '1',
						name: '男'
					},
					{
						value: '2',
						name: '女'
					}
				],
				current: 0,
				Name: ""
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
			name() {
				this.one = false
				let that = this
				uni.getStorage({
					key: 'dengLu',
					success: function(res) {
						uni.request({
							url: 'http://192.168.1.84:8081/m.api', //仅为示例，并非真实接口地址。
							data: {
								_gp: 'user',
								_mt: 'syncUserInfo',
								nickname: that.Name
							},
							header: {
								"Content-type": "application/x-www-form-urlencoded",
								"accesstoken": res.data
							},
							success: (resd) => {
								that.res = res.data.errmsg
							}
						});
					}
				});
			},
			sex() {
				this.two = false
				let that = this
				uni.request({
					url: 'http://192.168.1.84:8081/m.api', //仅为示例，并非真实接口地址。
					data: {
						_gp: 'user',
						_mt: 'syncUserInfo',
						gender: that.current
					},
					header: {
						"Content-type": "application/x-www-form-urlencoded",
						accesstoken: '5F8A81E49B204B19AC9D807AF76FE472'
					}
				});
			}
		}
	}
</script>

<style>
	.about>view>text:first-child {
		float: left;
	}

	.about>view>text:last-child {
		float: right;
		color: #999999;
	}

	.about {
		margin-bottom: 20vh;
	}

	.about>view {
		width: 95%;
		display: flow-root;
		height: 2rem;
		padding: 5px 10px;
		border-bottom: #C0C0C0 solid 1px;
	}

	.gai {
		width: 100%;
		height: 100vh;
		position: fixed;
		z-index: 999;
		background: rgba(0, 0, 0, 0.5);
		top: 0;
		box-sizing: border-box;
	}

	.gai>view {
		width: 70%;
		height: 21vh;
		margin: auto;
		margin-top: 33vh;
		background-color: white;
		border-radius: 1rem;
	}

	.gai>view>text:first-child {
		text-align: center;
		display: block;
		line-height: 8vh;
		border-bottom: #C0C0C0 solid 1px;
	}

	.gaI>text {
		display: inline-block;
		width: 50%;
		text-align: center;
		line-height: 4vh;
		box-sizing: border-box;
		line-height: 9vh;
	}
	.gaI{
		border-top: #C0C0C0 solid 1px;
	}
	.gaI>text:first-child{
		border-right: #C0C0C0 solid 1px;
	}
	.gai input{
		padding-left: 10px;
	}
</style>
