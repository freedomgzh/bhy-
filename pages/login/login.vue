<template>
	<view class="login">
		<view class="title">
			欢迎登陆
		</view>
		<view class="tips">
			未注册的手机号验证后自动创建账户
		</view>
		<view class="box">
			<view class="inputbox flexYc">
				<input type="number"  @input='listenPhone' v-model="phone" placeholder="请输入手机号" />
			</view>
			<view class="inputbox flexXb flexYc">
				<input type="number"  @input='listenCode' v-model="code" placeholder="请输入验证码" />
				<view class="label" @tap="getcode" v-if="!hasCode">
					获取验证码
				</view>
				<view class="label" v-else>
					{{timer}} s
				</view>
			</view>
			
		</view>
		<view class="btns flexYc flexXc " @tap="login">
			<view>登陆</view>
			
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				phone:'',
				code:'',
				realcode:'',
				hasCode:false,
				timer:60,
				setTimer:''
			}
		},
		methods:{
			listenPhone(e){
				this.phone = e.detail.value
			},
			listenCode(e){
				this.code = e.detail.value
			},
			async login(){
				if(!this.phone){
					uni.showToast({
						title:'请输入账号'
					})
					return
				}
				if(!this.code){
					uni.showToast({
						title:'请输入密码'
					})
					return
				}
				const postData={
					mobile:this.phone,
					code:this.code
					
				}
				const r = await  this.$api.login(postData)
				console.log('r===========',r)
				if(r.data.Status == 1){
					uni.setStorageSync('userInfo',r.data.Data)
					uni.switchTab({
						url:'../index/index'
					})
				}else{
					uni.showToast({
						title:r.data.Memo,
						icon:'none'
					})
				}
				
			},
			async getcode(){
				const r = await this.$api.GetMobileCode({mobile:this.phone})
				if(r.data.Status == 1){
					uni.showToast({
						title:r.data.Memo,
						icon:'none'
					})
					this.hasCode  = true;
					this.setTimer = setInterval(
					()=>{
						this.timer--
						if(this.timer == 0){
							clearInterval(this.setTimer)
							this.hasCode = false
						}
					},1000)
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
	.login{
		height: 100%;
		padding-top: 210upx;
		padding-left: 59upx;
		padding-right: 59upx;
	}
	.title{
		font-size: 48upx;
		font-weight: 800;
	}
	.tips{
		margin-top: 33upx;
		font-size:24upx;
	}
	.box{
		margin-top: 46upx;
		color: #B8B8B8;
		font-size: 26upx;
	}
	.box .inputbox{
		height: 100upx;
		border-bottom: 2upx solid#EEEEEE;
	}
	.btns{
		background-color: #005EA1;
		height: 76upx;
		border-radius: 38upx;
		color: #FFFFFF;
		font-size: 30upx;
		margin-top: 80upx;
	}
</style>
