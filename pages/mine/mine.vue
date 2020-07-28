<template>
	<view class="content">
		<view class="top flexYc flexXb">
			<view class="">
				<image :src="datas.HeadImg" mode="" class="headLogo"></image>
				<text>{{datas.NickName}}</text>
			</view>
			
			<view class="set"@tap="toSet">
				编辑个人资料
			</view>	
		</view>
		<view class="order">
			<view class="tips flexXb">
				<view class="ons">
					我的订单
				</view>
				<view class="tns">
					查看全部订单 >
				</view>

			</view>
			<view class="orderList flexXb flexYc">
				<view class="labels flexCol  flexXa" @tap="goOrder(1)">
					<text class="num">
						{{datas.WeiJieCount}}
					</text>
					<text>未接单</text>
				</view>
				<view class="labels flexCol" @tap="goOrder(2)">
					<text class="num">
						{{datas.YiJieCount}}
					</text>
					<text>进行中</text>
				</view>
				<view class="labels flexCol" @tap="goOrder(3)">
					<text class="num">
						{{datas.WanChengCount}}
					</text>
					<text>已完成</text>
				</view>
			</view>
		</view>
		<view class="list">
			<view class=" flexXb flexYc label" @tap="toPerson">
				<view class="flexYc">
					<image src="../../static/person.png" mode="" class="personimg"></image>
					<text>平台认证</text>
				</view>
				<image src="../../static/arrow.png" mode="" class="arrow"></image>
			</view>
			<view class="flexXb flexYc label" @tap="phone">
				<view class="flexYc">
					<image src="../../static/kf.png" mode="" class="kefuimg"></image>
					<text>客服电话</text>
				</view>

				<image src="../../static/arrow.png" mode="" class="arrow"></image>
			</view>
			<view class="flexXb flexYc label" @tap="suggess">
				<view class="flexYc">
					<image src="../../static/suggess.png" mode="" class="suggessimg"></image>
					<text>意见反馈</text>
				</view>

				<image src="../../static/arrow.png" mode="" class="arrow"></image>
			</view>
			<view class="flexXb flexYc label" @tap="setting">
				<view class="flexYc">
					<image src="../../static/setting.png" mode="" class="settingimg"></image>
					<text>设置</text>
				</view>

				<image src="../../static/arrow.png" mode="" class="arrow"></image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				datas: {}
			}

		},
		onLoad() {
			this.user = this.$store.getters.userinfo
			this.getInfo()

		},
		methods: {
			async getInfo() {
				const r = await this.$api.GetOrderCount({
					userId: this.$store.getters.userinfo.id
				})
				if (r.data.Status == 1) {
					this.datas = r.data.Data
					console.log('datas=========', this.datas)
				} else {
					uni.showToast({
						title: r.data.Memo,
						icon: 'none'
					})
				}
			},
			toPerson() {
				console.log(1111111)
				uni.navigateTo({
					url: './psersonInfo'
				})
			},
			phone() {
				console.log(1111111)
				uni.navigateTo({
					url: './phone'
				})
			},
			setting() {
				console.log(1111111)
				uni.navigateTo({
					url: './setting'
				})
			},
			suggess() {
				console.log(1111111)
				uni.navigateTo({
					url: './suggess'
				})
			},
			goOrder(id){
				uni.navigateTo({
					url:'../orderList/orderList/orderList?type=' + id
				})
			},
			toSet(){
				uni.navigateTo({
					url:'./setInfo'
				})
			}
		}
	}
</script>

<style>
	page {
		height: 100%;
	}

	.content {
		height: 100%;
		background-color: #F6F6F6;
	}

	.top {
		height: 210upx;
		background-color: #187BCD;
		color: #FFFFFF;
		font-size: 34upx;
	}

	.headLogo {
		border-radius: 50%;
		height: 110upx;
		width: 110upx;
		margin-right: 20upx;
		margin-left: 30upx;
	}

	.list {
		font-size: 28upx;
		background-color: #FFFFFF;
		border-radius: 8upx;
		margin: 20upx;
	}

	.label {
		height: 105upx;
		border-bottom: 2upx solid#EEEEEE;
	}

	.personimg {
		height: 46upx;
		width: 40upx;
		margin-right: 21upx;
		margin-left: 30upx;
	}

	.kefuimg {
		height: 41upx;
		width: 40upx;
		margin-right: 21upx;
		margin-left: 30upx;
	}

	.suggessimg {
		height: 42upx;
		width: 40upx;
		margin-right: 21upx;
		margin-left: 30upx;
	}

	.settingimg {
		height: 42upx;
		width: 41upx;
		margin-right: 21upx;
		margin-left: 30upx;
	}

	.arrow {
		width: 14upx;
		height: 25upx;
		margin-right: 20upx;
	}

	/* 订单 */
	.order {
		margin: 20upx 20upx 0 20upx;
		background-color: #FFFFFF;
		border-radius: 8upx;
	}

	.label {
		text-align: center;
	}

	.tips {
		height: 88upx;
		line-height: 88upx;
		border-bottom: 2upx solid#EEEEEE;
		padding: 0 20upx;
	}

	.ons {
		font-size: 30upx;
	}

	.tns {
		font-size: 26upx;
		color: #666666;
	}

	.orderList {
		font-size: 28upx;
		height: 141upx;
		padding: 0 20upx;
	}

	.orderList text {
		text-align: center;
	}

	.num {
		font-size: 38upx;
	}
	.set{
		color: #FFFFFF;
		height: 50upx;
		width: 190upx;
		text-align: center;
		border: 2upx solid#FFFFFF;
		border-radius: 10upx;
		line-height: 50upx;
		font-size: 26upx;
		margin-right: 43upx;
	}
</style>
