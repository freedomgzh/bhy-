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
				<picker mode="time" :value="time" start="09:01" end="21:01" @change="bindTimeChange">
					<view class="uni-input">{{time}}</view>
				</picker>


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
	</view>
</template>

<script>
	import xflSelect from '../../components/xfl-select/xfl-select.vue';
	import sunUiUpimg from '@/components/sunui-upimg/sunui-upimg.vue';
	export default {
		components: {
			xflSelect,
			sunUiUpimg
		},
		data() {
			return {
				placeholder: '请选择工厂',
				listBoxStyle: `height: 40px; font-size: 16px;`,
				list: [],
				time:'',
				wupin:'',
				driver:'',
				phone:'',
				carNumber:''
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
			},
			bindTimeChange(e) {
				this.time = e.target.value
			},
			wupin(e){
				this.wupin = e.detail.value
			},
			driver(e){
				this.driver = e.detail.value
			},
			phone(e){
				this.phone = e.detail.value
			},
			carNumber(e){
				this.carNumber = e.detail.value
			},
		},
	}
</script>

<style>
	page {
		height: 100%;

	}

	.content {
		height: 100%;
		background-color: #F6F6F6;

		padding-top: 22upx;
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
	.uni-input{
		height: 100upx;
		width: 200upx;
		line-height: 100upx;
		text-align: right;
		font-size: 28upx;
		color: #CACACA;
		
	}
</style>
