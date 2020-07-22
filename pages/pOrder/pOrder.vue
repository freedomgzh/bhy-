<template>
	<view class="content">
		<view class="searchBox flexYc">
			<xfl-select :list="list" :clearable="true" :showItemNum="5" :isCanInput="true" :style_Container="listBoxStyle"
			 :placeholder="placeholder" @change="change" v-if="list.length != 0">
			</xfl-select>
		</view>
		<view class="listBox">
			<view class="list flexYc flexXb">
				<text>请选择到厂时间</text>
				
				<view class="uni-list-cell-db flex">
					<picker mode="date" :value="date" :start="startDate" :end="endDate" @change="bindDateChange">
						<view class="uni-input">{{date}}</view>
					</picker>
					<picker mode="time" :value="time" start="09:01" end="21:01" @change="bindTimeChange">
						<view class="uni-input">{{time}}</view>
					</picker>
				</view>

			</view>
			<view class="list flexYc flexXb">
				<text>运输物</text>
				<input type="text" @input="wupin" value="" placeholder-style="text-align:right" placeholder="请填写运输物" />
			</view>
			<view class="list flexYc flexXb">
				<text>驾驶员</text>
				<input type="text" value="" @input="driver" placeholder-style="text-align:right" placeholder="请填写驾驶员姓名" />
			</view>
			<view class="list flexYc flexXb">
				<text>随车电话</text>
				<input type="text" value="" @input="phone" placeholder-style="text-align:right" placeholder="请填写随车电话" />
			</view>
			<view class="list flexYc flexXb">
				<text>车牌号</text>
				<input type="text" value="" @input="carNumber" placeholder-style="text-align:right" placeholder="请填写车牌号" />
			</view>
		</view>
		<sunui-upimg @change="getImageInfo1" :upload_auto="true" ref="upimg1" :upload_count="2"></sunui-upimg>
		<view class="title">
			请选择监督工作项
		</view>
		<view class="bg">
			<view class="line flexXb flexYc">
				<text>清罐：</text>
				<jiuaiCheckbox @change='changeBox1' value="自定义的一个value" :checked="true" :checkBoxSize='40' :borderRadius='30'></jiuaiCheckbox>
			</view>
			<view class="line flexXb flexYc">
				<text>铅封：</text>
				<jiuaiCheckbox @change='changeBox2' value="自定义的一个value" :checked="true" :checkBoxSize='40' :borderRadius='30'></jiuaiCheckbox>
			</view>
		</view>
		<view class="btnsubmit" @tap="freeTell">
			立即发布
		</view>
	</view>
</template>

<script>
	import xflSelect from '../../components/xfl-select/xfl-select.vue';
	import sunUiUpimg from '@/components/sunui-upimg/sunui-upimg.vue';
	import jiuaiCheckbox from "@/components/jiuai-checkbox/jiuai-checkbox.vue";

	export default {
		components: {
			xflSelect,
			sunUiUpimg,
			jiuaiCheckbox
		},
		data() {
			const currentDate = this.getDate({
				format: true
			})
			return {
				placeholder: '请选择工厂',
				listBoxStyle: `height: 40px; font-size: 16px;`,
				list: [],
				time: '00:00',
				goodsName: '',
				driverName: '',
				mobile: '',
				carNumbers: '',
				factoryId: '',
				date: currentDate,
				carImgs:[],
				checkOne:false,
				checkTwo:false,
				workContent:0,
				userId:uni.getStorageSync('userInfo').id
			}
		},
		onLoad() {
			this.getList()
		},
		methods: {
			async getList() {
				const r = await this.$api.GetFactoryList()
				if (r.data.Status == 1) {
					this.list = r.data.Data.Rows

					this.list.forEach((s, i) => {
						s.value = s.name
					})
					console.log('r=========', r, this.list.length)
				} else {
					uni.showToast({
						title: r.data.Memo,
						icon: 'none'
					})
				}
			},
			change({
				newVal,
				oldVal,
				index,
				orignItem
			}) {
				console.log(newVal, oldVal, index, orignItem);
				this.factoryId = orignItem.id
			},
			bindTimeChange(e) {
				this.time = e.target.value
			},
			wupin(e) {
				this.goodsName = e.detail.value
			},
			driver(e) {
				this.driverName = e.detail.value
			},
			phone(e) {
				this.mobile = e.detail.value
			},
			carNumber(e) {
				this.carNumbers = e.detail.value
			},
			//picurl
			getImageInfo1(e) {
				console.log('图片返回1：', e)
				this.carImgs = e
			},
			changeBox1(e) { //选中切换事件(由组件发起)
				console.log('点击了checkBox', e);
				this.checkOne = e.detail.checked;
				
			},
			changeBox2(e) { //选中切换事件(由组件发起)
				console.log('点击了checkBox', e);
				this.checkTwo = e.detail.checked;
				
			},
			bindDateChange: function(e) {
				this.date = e.target.value
			},
			// tijiaofabu
			async freeTell(){
				if(this.checkOne && this.checkTwo){
					this.workContent = 2
				}else if(!this.checkOne && this.checkTwo){
					this.workContent = 1
				}if(this.checkOne && !this.checkTwo){
					this.workContent = 0
				}
				console.log('===============',this.userId)
				const data = {
					userId:this.userId,
					factoryId:this.factoryId,
					daochangTime:this.date +  " " + this.time + ":00",
					goodsName:this.goodsName,
					driverName:this.driverName,
					mobile:this.mobile,
					carNumber:this.carNumbers,
					
					carImgs:this.carImgs.join(';') ,
					workContent:this.workContent
				}
				const r = await this.$api.CreateOrder(data)
				console.log('r==============',r)
				if (r.data.Status == 1) {
					this.orderId = r.data.Data.orderId
					this.getopenid()
				} else {
					uni.showToast({
						title: r.data.Memo,
						icon: 'none'
					})
				}
			},
			getDate(type) {
				const date = new Date();
				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();

				if (type === 'start') {
					year = year - 60;
				} else if (type === 'end') {
					year = year + 2;
				}
				month = month > 9 ? month : '0' + month;;
				day = day > 9 ? day : '0' + day;
				return `${year}-${month}-${day}`;
			},
			async  getopenid(){
				var that = this;
				const loginRes   =  await  uni.login({
				  provider: 'weixin'
				});
				console.log('loginRes===',loginRes)
				const r = await that.$api.GetOpenId({code:loginRes[1].code});
				console.log('r=====================',r)
				if(r.data.Status == 1){
					that.openId=r.data.Data.openId
					
				}else{
					uni.showToast({
						title:r.data.Memo,
						icon:'none'
					})
				}
				this.wepay()
			},
			async wepay(){
				var that = this;
				const r = await this.$api.WeChatPay({
					userId:uni.getStorageSync('userInfo').id,
					openId:that.openId,
					orderId:that.orderId
				})
				console.log('r============',r,that.openId)
				if(r.data.Status == 1){
						
					this.appid = r.data.Data.appid
					this.noncestr=r.data.Data.noncestr
					this.package=r.data.Data.package
					this.partnerid=r.data.Data.partnerid
					this.prepayid=r.data.Data.prepayid
					this.sign=r.data.Data.sign
					this.timestamp= '' +　r.data.Data.timestamp
					this.payHandler()
				}else{
					uni.showToast({
						title:r.data.Memo,
						icon:'none'
					})
				}
			},
			// 支付按钮点击方法
			payHandler () {
				
				 console.log(3333)
				// await  this.wepay()
				uni.req
				var that = this;
				console.log('=======',that.paySign)
				// #ifdef MP-WEIXIN
				uni.requestPayment({
					provider: 'wxpay',
					orderInfo: 'orderInfo', // 订单数据
					timeStamp: '' + that.timestamp, // 时间戳从1970年1月1日至今的秒数，即当前的时间
					nonceStr:  that.noncestr, // 随机字符串，长度为32个字符以下
					package:   that.prepayid, // 统一下单接口返回的 prepay_id 参数值，提交格式如：prepay_id=xx
					signType: 'MD5', //签名算法，暂支持 MD5
					paySign: that.sign, // 签名
					success: function (res) {
						// 支付成功的回调中 创建绘本馆成功
						uni.showToast({
							title: '微信支付成功',
							icon: 'success',
							duration: 1500
						});
					},
					fail: function (err) {
						// 支付失败的回调中 用户未付款
						console.log('err===========',err)
						uni.showToast({
							title: '支付取消',
							duration: 1500,
							image: '/static/png/error_icon.png'
						});
					}
				});
				// #endif
				// #ifdef MP-ALIPAY
				uni.requestPayment({
					provider: 'alipay',
					orderInfo: '', // 订单数据
					success: function (res) {
						if (res.resultCode == 6001) {
							uni.showToast({
								title: '支付取消',
								icon: 'none',
								duration: 1500
							});
						} else {
							uni.showToast({
								title: '支付宝支付成功',
								icon: 'success',
								duration: 1500
							});
						}
					},
					fail: function (err) {
						// 支付失败的回调中 用户未付款
						uni.showToast({
							title: '支付取消',
							duration: 1500,
							icon: 'none'
						});
					}
				});
				// #endif
			}
		
		},
		
	}
</script>

<style>
	page {}

	.content {

		background-color: #F6F6F6;

		padding-top: 22upx;
	}

	.bg {
		background-color: #FFFFFF;

	}

	.searchBox {
		height: 150upx;
		padding: 0 20upx;
		background-color: #FFFFFF;

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

	.uni-input {
		height: 100upx;
		width: 200upx;
		line-height: 100upx;
		text-align: right;
		font-size: 28upx;
		color: #CACACA;

	}

	.title {
		font-size: 26upx;
		height: 65upx;
		line-height: 65upx;
		background-color: #EEEEEE;
		margin-left: 18upx;
	}

	.line {
		height: 110upx;
		margin: 0 20upx;
		border-bottom: 2upx solid#DCDCDC;
		background-color: #FFFFFF;
		font-size: 30upx;
	}

	.btnsubmit {
		position: absolute;
		/* bottom: 60upx; */
		left: 20upx;
		height: 84upx;
		width: 710upx;
		border-radius: 10upx;
		color: #FFFFFF;
		text-align: center;
		line-height: 84upx;
		background-color: #005EA1;
		font-size: 32upx;
		margin: 41upx 0;
	}
</style>
