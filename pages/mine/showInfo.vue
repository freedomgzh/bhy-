<template>
	<view class="content">
		<view class="listBox">
			<view class="list flexYc flexXb">
				<text>加油站</text>
				<input type="text" disabled="true" @input="wupin" value="" placeholder-style="text-align:right" :placeholder="data.siteName" />
			</view>
			<view class="list flexYc flexXb">
				<text>负责人</text>
				<input type="text" disabled="true" value="" @input="driver" placeholder-style="text-align:right" :placeholder="data.manager" />
			</view>
			<view class="list flexYc flexXb">
				<text class="phone">手机号</text>
				<input type="text" value="" disabled="true" @input="phone" placeholder-style="text-align:right" :placeholder="data.mobile" />
			</view>
			
		</view>
		<view class="title">
			营业执照
		</view>
		<image :src="data.img" mode=""></image>
		
	</view>
</template>

<script>
	import sunUiUpimg from '@/components/sunui-upimg/sunui-upimg.vue';
	export default{
		components: {
			sunUiUpimg
		},
		data(){
			return{
				data:{}
				// goodsName:'',
				// driverName:'',
				// mobile:''
			}
			
		},
		onLoad(e) {
			console.log(1111111111)
			this.data = JSON.parse(e.data)
			this.user = uni.getStorageSync('userInfo').id
		},
		methods:{
			wupin(e) {
				this.goodsName = e.detail.value
			},
			driver(e) {
				this.driverName = e.detail.value
			},
			phone(e) {
				this.mobile = e.detail.value
			},
			getImageInfo1(e) {
				console.log('图片返回1：', e)
				this.img = e
			},
			async sub(){
				
				const data = {}
				data.userId = uni.getStorageSync('userInfo').id;
				data.manager = this.driverName
				data.mobile = this.mobile
				data.siteName = this.goodsName
				data.img = this.img.splice()
				const r = await this.$api.RenZheng(data)
				console.log('r=============',r)
				if(r.data.Status == 1){
					uni.switchTab({
						url:'./mine'
					})
					
				}else{
					uni.showToast({
						title:r.data.Memo,
						icon:'none'
					})
				}
			}
		}
	}
</script>

<style>
	page{
		height: 100%;
	}
	.content{
		height: 100%;
		background-color: #F6F6F6;
		padding-top: 19upx;
	}
	.listBox {
		background-color: #FFFFFF;
	}
	
	.list {
		height: 110upx;
		margin: 0 20upx;
		border-bottom: 2upxsolid#EEEEEE;
	}
	
	.list text {
		font-size: 30upx;
	}
	
	.list input {
		font-size: 28upx;
		color: #CACACA;
	}
	.title{
		font-size: 30upx;
		height: 50upx;
		padding-left: 20upx;
		line-height: 50upx;
		background-color: #FFFFFF;
	}
	.phone{
		margin-right: 10upx;
	}
</style>
