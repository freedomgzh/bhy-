<template>
	<view class="content">
		<view class="listBox">
			<view class="list flexYc" v-for="(s,i) in list" :key="i">
				<image :src="s.img" mode="" class="logoImg"></image>
				<view class="flexCol">
					<text class="name">{{s.name}}</text>
					<text class="address">地址：{{s.address}}</text>
				</view>
			</view>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
				list: [],
				
			}
		},
		onLoad() {
			this.getList();
			
		},
		
		methods: {
			async getList(v) {
				const r = await this.$api.GetFactoryList()
				console.log('r=========', r)
				if (r.data.Status == 1) {
					this.list = r.data.Data.Rows
					
				} else {
					uni.showToast({
						title: r.data.Memo,
						icon: 'none'
					})
				}
			},
			
			
		}
	}
	
</script>

<style>
	page {
		height: 100%;
	}
	
	.content {
		
		height: 100%;
		/* 	display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center; */
	}
	
	.top {
		height: 300upx;
		width: 100%;
	}
	.search{
		margin: 25upx 20upx 30upx 20upx;
		height: 80upx;
		border-radius: 40upx;
		box-shadow: 5upx 5upx 15upx  rgba(24,123,205,0.1);
		font-size: 28upx;
	}
	.spic{
		width: 36upx;
		height: 36upx;
		margin-left: 60upx;
		margin-right: 10upx;
		
	}
	.title{
		margin: 30upx 20upx;
	}
	.o{
		font-size: 32upx;
		
	}
	.n{
		font-size: 26upx;
		color: #999999;
	}
	.list{
		margin:0 18upx;
		border-bottom: 2upx solid#EEEEEE;
		height: 140upx;
	}
	.logoImg{
		width:100upx;
		height: 100upx;
		
		margin-right: 20upx;
	}
	.name{
		
		font-weight: 400;
	}
	.address{
		font-size: 26upx;
	}
	.btns{
		width: 146upx;
		height: 146upx;
		position: absolute;
		right: 	32upx;
		bottom: 46upx;
	}
	.aimg{
		height: 300upx;
		width: 100%;
	}
</style>
