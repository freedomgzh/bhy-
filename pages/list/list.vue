<template>
	<view class="content">
		<view class="search flexYc">
			<image src="../../static/serach.png" mode="" class="spic"></image>
			<input  type="text" @input="search" placeholder-style="color: #DDDDDD;" value="" placeholder="搜索厂家" class="inputs" />
		</view>	
		<view class="listBox">
			<view class="list flexYc"  @tap="toOrder(s.id)" v-for="(s,i) in list" :key="i">
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
				list:[],
				timer:''
			}
		},
		methods: {
			search(e){
				var that  = this;
				console.log(e)
				if(that.timer){
					clearTimeout(that.timer)
				}
				this.timer =  setTimeout(()=>{
					that.getlist(e.detail.value)
				},1000)
				
			},
			async getlist(name){
				const r  =await  this.$api.GetFactoryList({name})
				console.log('r==========',r)
				if (r.data.Status == 1) {
					this.list = r.data.Data.Rows
					
				} else {
					uni.showToast({
						title: r.data.Memo,
						icon: 'none'
					})
				}
			},
			toOrder(id){
				uni.navigateTo({
					url:'../pOrder/pOrderi?id=' + id
				})
			},
		}
	}
</script>

<style>
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
</style>
