<template>
	<view>
		<view class="about">
			<view><text>版本号</text><text>v1.0.0</text></view>
			<view><text>贡献</text><text>sixblog</text></view>
			<view><text>邮箱</text><text>sixblog@163.com</text></view>
		</view>
		<text id="about">由sixblog开发</text>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>
.about>view>text:first-child{
	float: left;
}
.about>view>text:last-child{
	float: right;
	color: #999999;
}
.about{
	margin-bottom: 20vh;
}
.about>view{
	width: 95%;
	display: flow-root;
	height: 2rem;
	padding: 5px 10px;
	border-bottom: #C0C0C0 solid 1px;
}
#about{
	display: table;
	margin: auto;
	clear: both;
	color: #C0C0C0;
}
</style>
