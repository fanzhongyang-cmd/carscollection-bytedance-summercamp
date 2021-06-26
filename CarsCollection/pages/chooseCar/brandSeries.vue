<template>
	<view class="content">
		<view class="tabs">
			<u-tabs :list="tabList" :is-scroll="false" :current="currentTab" active-color="#000000" @change="changeTab">
			</u-tabs>
		</view>

		</u-tabs>
		<view class="tags">
			<view v-for="(tag,index) in tagList" :key="index" @click="clickTag(index)">
				<u-tag class="tag" :text="tag" mode="light" color="#000000"
					:bg-color="currentTag==index?'#FFFFFF':'#F7F8FC'"
					:border-color="currentTag==index?'#606370':'#F7F8FC'" shape="square" />
			</view>
		</view>

		<view class="content">
			<view v-for="(series,index) in showSeriesList">
				<view class="card-series" @click="clickSeries({id:series.id,name:series.name})">
					<image class="card-series-image" :src="series.image"></image>
					<view class="card-series-info">
						<view class="card-serires-name">{{series.name}}</view>
						<view class="card-serires-stars">{{series.stars}}</view>
						<view class="card-serires-price">指导价：{{series.minPrice}}万起</view>
					</view>
					<view class="card-serires-button" @click.stop="clickButton(index)">试驾</view>
				</view>
			</view>
		</view>

		<u-popup v-model="show" mode="bottom" height="40%" border-radius="16">
			<view class="popup">

				<image class="popup-close" @click="closePopup" src="/static/icon_close.png"></image>

				<view class="popup-title">
					立即预约试驾体验
				</view>
				<view class="popup-explian">
					提交信息后工作人员会在3个工作日内与您联系
				</view>
				<u-field class="popup-form" v-model="city" :disabled="true" label="城市" placeholder="请选择城市">
					<image slot="right" src="/static/list_arrow_ic.png" style="width: 24rpx;height: 24rpx;"
						@click="chooseCity"></image>
				</u-field>
				<u-field class="popup-form" v-model="name" label="姓名" placeholder="请输入您的姓名">
				</u-field>
				<u-field class="popup-form" v-model="phone" label="手机号" placeholder="请输入您的手机号">
				</u-field>
				<view class="popup-submit" @click="clickSubmit">
					提交
				</view>
			</view>
		</u-popup>



	</view>
</template>

<script>
	export default {
		data() {
			return {
				currentTab: 0,
				tabList: [{
					name: '在售'
				}, {
					name: '未售/停售'
				}],

				currentTag: 0,
				tagList: ['全部','轿车','SUV','MPV'],

				showSeriesList: [{
					id:'123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马1系',
					stars: 3,
					minPrice: 28.46,
					tag: '轿车',
					halted: false,
				}, {
					id:'123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马2系',
					stars: 3,
					minPrice: 28.46,
					tag: 'SUV',
					halted: false,
				}, {
					id:'123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马2系旅行车新能源(进口)',
					stars: 3,
					minPrice: 28.46,
					tag: 'MPV',
					halted: false,
				}, ],
				allSeriesList: [{
					id:'123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马1系',
					stars: 3,
					minPrice: 28.46,
					tag: '轿车',
					halted: false,
				}, {
					id:'123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马2系',
					stars: 3,
					minPrice: 28.46,
					tag: 'SUV',
					halted: false,
				}, {
					id:'123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马2系旅行车新能源(进口)',
					stars: 3,
					minPrice: 28.46,
					tag: 'MPV',
					halted: false,
				}, {
					id:'123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马2系旅行车新能源(进口)',
					stars: 3,
					minPrice: 88.88,
					tag: 'MPV',
					halted: true,
				}],

				show: false,
				testdrive: '',
				city: '',
				name: '',
				phone: ''
			}
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数
			console.log(option.id); //打印出上个页面传递的参数。
			console.log(option.name); //打印出上个页面传递的参数。

			if (option.name) {
				uni.setNavigationBarTitle({
					title: option.name
				})
			}
		},
		methods: {
			changeTab(e) {
				this.currentTab = e;
				this.currentTag = 0;
				let requestTab = e==0?false:true;
				let showSeriesList = this.allSeriesList.filter( item =>{
					return item.halted == requestTab
				});
				this.showSeriesList = showSeriesList;
			},
			clickTag(e) {
				console.log(e);
				if (this.currentTag != e) {
					this.currentTag = e;
					let requestTab = this.currentTab==0?false:true;
					if(e==0){
						this.changeTab(this.currentTab);
					}else{
						let requestTag = this.tagList[e];
						let showSeriesList = this.allSeriesList.filter( item =>{
							return item.tag == requestTag && item.halted == requestTab;
						});
						this.showSeriesList = showSeriesList;
					}
				}
			},
			clickSeries(e) {
				console.log(e)
				let option = '?id=' + e.id +'&name='+e.name;
				uni.navigateTo({
					url: '/pages/chooseCar/seriesCar' + option,
				})
			},
			clickButton(e) {
				console.log(e);
				this.testdrive = e;
				this.show = true;
			},
			clickSubmit(e) {
				console.log(this.city, this.name, this.phone);
			},
			closePopup(e) {
				this.show = false;
			},
			chooseCity(e) {
				let _this = this;
				uni.navigateTo({
					url: '/pages/chooseCar/chooseCity',
					events: {
						acceptDataFromOpenedPage: function(data) {
							console.log(data);
							_this.city = data.city;
						},
					}
				})
			}
		},
	}
</script>

<style>
	.content {
		width: 100%;
		display: flex;
		flex-direction: column;
	}

	.tabs {
		width: 350rpx;
	}

	.tags {
		padding: 24rpx 28rpx;
		display: flex;
		flex-direction: row;
		justify-content: flex-start;
	}

	.tag {
		padding: 8rpx;
	}

	.cards {
		width: 100%;
		height: 192rpx;
		background: #FFFFFF;
		display: flex;
		flex-direction: column;
	}

	.card-series {
		display: flex;
		flex-direction: row;
		justify-content: flex-start;
		align-items: center;
	}

	.card-series-image {
		flex-shrink: 0;
		width: 200rpx;
		height: 132rpx;
		padding: 24rpx 48rpx;
	}

	.card-series-info {
		flex-grow: 0;
		display: flex;
		flex-direction: column;
	}

	.card-serires-name {
		padding: 6rpx 0;
		font-family: PingFangSC-Medium;
		font-size: 32rpx;
		color: #1A1A1A;
		letter-spacing: 0;
		font-weight: 500;
	}

	.card-serires-stars {
		padding: 6rpx 0;
		font-family: Helvetica;
		font-size: 24rpx;
		color: #1F2129;
		line-height: 24rpx;
		font-weight: 400;
	}

	.card-serires-price {
		padding: 6rpx 0;
		font-family: PingFangSC-Regular;
		font-size: 24rpx;
		color: #999999;
		letter-spacing: 0;
		line-height: 36rpx;
	}

	.card-serires-button {
		flex-shrink: 0;

		margin-left: auto;
		margin-right: 32rpx;
		width: 132rpx;
		height: 56rpx;
		display: flex;
		justify-content: center;
		align-items: center;

		background: #FFCC32;
		border-radius: 4rpx;

		font-family: PingFangSC-Regular;
		font-size: 28rpx;
		color: #1A1A1A;
		letter-spacing: 0;
		text-align: center;
		font-weight: 400;
	}

	.popup {
		display: flex;
		flex-direction: column;
		align-items: center;
		width: 100%;
	}

	.popup-close {
		width: 32rpx;
		height: 32rpx;
		position: absolute;
		right: 32rpx;
		top: 32rpx;
	}

	.popup-title {
		padding-top: 32rpx;
		font-family: PingFangSC-Medium;
		font-size: 32rpx;
		color: #1A1A1A;
		text-align: center;
		line-height: 44rpx;
		font-weight: 500;
	}

	.popup-explian {
		padding: 16rpx;
		font-family: PingFangSC-Regular;
		font-size: 24rpx;
		color: #666666;
		text-align: center;
		line-height: 36rpx;
		font-weight: 400;
	}

	.popup-form {
		width: 686rpx;
	}

	.popup-submit {
		width: 686rpx;
		height: 88rpx;

		background: #FFCD32;
		border-radius: 4rpx;

		display: flex;
		justify-content: center;
		align-items: center;

		font-family: PingFangSC-Medium;
		font-size: 32rpx;
		color: #1F2129;
		letter-spacing: 0;
		text-align: center;
		line-height: 32rpx;
		font-weight: 500;
	}
</style>
