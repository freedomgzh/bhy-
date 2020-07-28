<template>
	<view class="content">
		<view class="f flexYc flexXb">
			<view class="">
				头像
			</view>
			<image src="../../static/相机%20(1)%20拷贝@3x.png" mode="" @tap="upLoad"></image>
		</view>
		<view class="t flexYc flexXb">
			<view class="">
				昵称
			</view>
			<input type="text" :value="datas.NickName" @input="listenname" />
		</view>
		<view class="btns" @tap="sub">
			提交
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				datas:{},
				name:'',
				img:''
			}
		},
		methods:{
			async GetOrderCount(){
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
			 listenname(e){
				this.name = e.detail.value
			},
			async sub(){
				const r = await this.$api.EditInfo({
					userId: this.$store.getters.userinfo.id,
					nikeName:this.name,
					avatarImg:this.img
				})
				if (r.data.Status == 1) {
					this.datas = r.data.Data
					console.log('datas=========', this.datas)
					uni.switchTab({
						url:'mine'
					})
				} else {
					uni.showToast({
						title: r.data.Memo,
						icon: 'none'
					})
				}
			},
			async  upLoad(i) {
				var self = this
				const res = await uni.chooseImage({
					count: 1, // 默认9
					sizeType: ['original', 'compressed'], // 可以指定是原图还是压缩图，默认二者都有
					sourceType: ['album', 'camera'], // 可以指定来源是相册还是相机，默认二者都有
					// success: function (res) {
					// 返回选定照片的本地文件路径列表，tempFilePath可以作为img标签的src属性显示图片
					//   var tempFilePaths = res.tempFilePaths;
					//   self.imgSrc = tempFilePaths;
					//   console.log('tempFilePaths', tempFilePaths);
			
					//   uni.showLoading({
					// 	title: '上传中',
					//   })
					//    console.log(formApi.header);
			
					// 	},
					// 	fail: function (res) {
					// 	  uni.hideLoading()
					// 	  console.log(res.data)
					// 	}
				})
				console.log('img==============================', res)
				if (res[1].errMsg = "chooseImage:ok") {
					var tempFilePaths = res[1].tempFilePaths;
					self.imgSrc = tempFilePaths;
					console.log('tempFilePaths', tempFilePaths);
			
					uni.showLoading({
						title: '上传中',
					})
					
					const r = await uni.uploadFile({
						url: `${self.$api.picUrl}/upload.ashx`,
						fileType: "image",
						filePath: self.imgSrc[0] + "",
						// filePath:'',
						name: 'file',
						formData: {
							filePath: self.imgSrc[0] + "",
						},
						header: {
							'Content-Type':'multipart/form-data'
						},
					})
					console.log('r===============',r)
					if (r[1].errMsg = "chooseImage:ok") {
						console.log('上传商品图片res.data.pic_url', r)
			
						const data = JSON.parse(r[1].data)
						if (data.Status == 1) {
							//   const index = data.data.indexOf('/static');
							//   self.imgUrl =   data.data.substr(index+1,r[1].data.length)
							self.img = data.Data.path
							uni.showToast({
								title: '图片上传成功',
							})
							uni.hideLoading()
						} else {
							uni.showToast({
								title:data.Memo
							})
							uni.hideLoading()
							
						}
					}
				} else {
					uni.hideLoading()
					console.log(res.data)
			
				}
			},
		},
		onLoad(e) {
			this.name = e.name
			this.GetOrderCount()
		}
	}
</script>

<style>
	.f{
		margin: 0 20upx;
		height: 147upx;
		border-bottom:2upx solid#DCDCDC;
		font-size: 28upx;
	}
	.f image{
		width: 106upx;
		height: 106upx;
		margin-right: 10upx;
	}
	.t{
		margin: 0 20upx;
		height: 88upx;
		
		font-size: 28upx;
	}
	.t input{
		text-align: right;
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
