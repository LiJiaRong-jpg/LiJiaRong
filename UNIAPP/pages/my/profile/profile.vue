<template>
	<view>
		
		<input type="text" v-model="Name" placeholder="修改后的名称"  />
		<button type=" warn" @click="name">确定修改名称</button>
		
		<view class="uni-list">
			<radio-group @change="radioChange">
				<label class="uni-list-cell uni-list-cell-pd" v-for="(item, index) in items" :key="item.value" style="float: left;">
					<view>
						<radio :value="item.value" :checked="index === current" />
					</view>
					<view>{{item.name}}</view>
				</label>
			</radio-group>
		</view>
		<button type=" warn" @click="sex" style="clear: both;">确定修改性别</button>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
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
				Name:""
			}
		},
		methods: {
			radioChange: function(evt) {
				for (let i = 0; i < this.items.length; i++) {
					if (this.items[i].value === evt.target.value) {
						this.current = i;
						console.log(this.current)
						break;
					}
				}
			},
			name(){
				let that = this
				uni.request({
					url: 'http://192.168.1.84:8081/m.api', //仅为示例，并非真实接口地址。
					data: {
						_gp: 'user',
						_mt: 'syncUserInfo',
						nickname:that.Name
					},
					header: {
						"Content-type": "application/x-www-form-urlencoded",
						accesstoken: '5F8A81E49B204B19AC9D807AF76FE472'
					},
					success: (res) => {
						console.log(res.data);
						that.res=res.data.errmsg
					}
				});
			},
			sex(){
				let that = this
				uni.request({
					url: 'http://192.168.1.84:8081/m.api', //仅为示例，并非真实接口地址。
					data: {
						_gp: 'user',
						_mt: 'syncUserInfo',
						gender:that.current
					},
					header: {
						"Content-type": "application/x-www-form-urlencoded",
						accesstoken: '5F8A81E49B204B19AC9D807AF76FE472'
					},
					success: (res) => {
						console.log(res.data);
					}
				});
			}
		}
	}
</script>

<style>

</style>
