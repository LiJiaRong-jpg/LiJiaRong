<template>
	<view>
		<view class="back">
			<view style="margin-left: 2rem;margin-top: 20vh;">欢迎回来！</view>
			<view style="margin-top: 5vh;" class='deng' v-if="shouJi==true">
				<view>微信快速登录(推荐)</view>
				<view @click="shou">手机注册登录</view>
			</view>
			<view style="padding-top: 5vh;margin:auto;width: 80%;" v-if="shouJi==false">
				<view style="background-color: #F8F6Fb;width: 100%;height: 3rem;margin-bottom: 1rem;">
					<text style="padding-left: 10px;">手机号码</text>
					<input
						style="margin-bottom: 1vh;width: 100%;border-radius: 1rem;padding-left: 10px;box-sizing: border-box;"
						maxlength="11" type="number" v-model="phone" placeholder="请输入手机号码" class="uni-input" />
				</view>
				<view style="background-color: #F8F6Fb;width: 100%;height: 3rem;margin-bottom: 1rem;">
					<text style="padding-left: 10px;">密码</text>
					<input style="width: 100%;border-radius: 1rem;padding-left: 10px;box-sizing: border-box;"
						type="text" v-model="Password" placeholder="8-18位不含特殊字符的数字、字母组合" maxlength="18" />
				</view>
				<text @click="denglu"
					style="width: 100%;margin: auto;line-height: 2rem;border-radius: 5rem;background-color: #fc4369;color: white;display: block;text-align: center;">登录</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				shouJi: true,
				phone: '13587878787',
				Password: 'asd123456'
			}
		},
		methods: {
			shou() {
				this.shouJi = !this.shouJi
			},
			denglu() {
				let arr=[]
				arr.push(this.phone)
				arr.push(this.Password)
				uni.setStorage({
				    key: 'phone',
				    data: arr
				});
				
				if (this.Password.length > 8) {
					let that = this
					uni.request({
						url: 'http://192.168.1.54:8081/m.api', //仅为示例，并非真实接口地址。
						data: {
							_gp: 'user',
							_mt: 'login',
							phone: that.phone,
							password: that.Password,
							ip: '1'
						},
						header: {
							"Content-type": "application/x-www-form-urlencoded"
						},
						success: (res) => {
							uni.setStorage({
								key: 'dengLu',
								data: res.data.data.accessToken
							});
							uni.setStorage({
								key: 'resd',
								data: res.data.data
							});
							uni.navigateBack({
								delta: 1
							});
						},
						complete() {
							that.dengLu = false
						}
					});
				}
			}
		}
	}
</script>

<style>
	.deng {
		width: 100%;
	}

	.deng>view {
		width: 80%;
		margin: auto;
		line-height: 2rem;
		border-radius: 5rem;
		background-color: #fc4369;
		color: white;
		padding: 0 1rem;
		box-sizing: border-box;
		text-align: center;
		margin-bottom: 1rem;
	}

	.back {
		height: 100vh;
		background: url('../../../static/91620812327_.pic_hd.jpg') no-repeat;
		overflow-y: hidden;
		overflow-x: hidden;
		width: 100%;
		background: url('../../../static/91620812327_.pic_hd.jpg') no-repeat;
		background-size: 100% auto;
		padding-top: 13%;
	}
</style>
