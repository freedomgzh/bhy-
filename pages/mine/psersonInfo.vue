<template>
	<view class="content">
		<view class="listBox">
			<view class="list flexYc flexXb">
				<text>加油站</text>
				<input type="text" @input="wupin" value="" placeholder-style="text-align:right" placeholder="请填写加油站名称" />
			</view>
			<view class="list flexYc flexXb">
				<text>负责人</text>
				<input type="text" value="" @input="driver" placeholder-style="text-align:right" placeholder="请填写负责人姓名" />
			</view>
			<view class="list flexYc flexXb">
				<text>手机号</text>
				<input type="text" value="" @input="phone" placeholder-style="text-align:right" placeholder="请填写手机号" />
			</view>
			
		</view>
		<view class="title">
			请上传营业执照
		</view>
		<sunui-upimg @change="getImageInfo1" :upload_auto="true" ref="upimg1" :upload_count="1"></sunui-upimg>
		<view class="btns" @tap="sub">
			认证
		</view>
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
				goodsName:'',
				driverName:'',
				mobile:''
			}
			
		},
		onLoad() {
			console.log(1111111111)
			this.user = this.$store.getters.userinfo
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
				data.userId = this.$store.getters.userinfo.id;
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
	.btns{
		position: absolute;
		bottom: 60upx;
		left: 20upx;
		height: 84upx;
		width: 710upx;
		border-radius: 10upx;
		color: #FFFFFF;
		text-align: center;
		line-height: 84upx;
		background-color: #005EA1;
		font-size: 32upx;
	}
</style>
