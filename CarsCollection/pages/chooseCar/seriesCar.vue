<template>
	<view>
		<view style="position: relative;">
			<image class="car-image" :src="carImageUrl"> </image>
			<image class="background" src="/static/形状结合.png"></image>
		</view>
		<view class="u-flex">
			<view class="flex-column">
				<view class="u-flex u-row-bottom">
					<view class="title">
						{{seriesName}}
					</view>
					<view class="description">
						{{brandName}}/{{seriesTag}}
					</view>
				</view>
				<view class="u-flex u-row-bottom">
					<view class="price">
						{{minPrice}}-{{maxPrice}}万
					</view>
					<view class="price-guide">
						指导价:{{minGuidePrice}}-{{maxGuidePrice}}万
					</view>
				</view>
			</view>
			<view @click="score">评分</view>
		</view>
		<view class="tags">
			<view v-for="(tag,index) in tagList" :key="index" @click="clickTag(index)">
				<u-tag class="tag" :text="tag" mode="light" color="#000000"
					:bg-color="currentTag==index?'#FFFFFF':'#F7F8FC'"
					:border-color="currentTag==index?'#606370':'#F7F8FC'" shape="square" />
			</view>
		</view>
		<view class="cars">
			<view v-for="(car,index) in showCarList" :key="index">
				<view class="u-flex u-row-center">
					<view class="flex-column">
						<view class="car-name">
							{{car.year}}款 {{car.name}}
						</view>
						<view class="u-flex u-row-center">
							<view class="car-price">{{car.price}}万起</view>
							<view class="car-price-guide">指导价:{{car.guideprice}}万</view>
						</view>
					</view>
					<view class="button-test-drive" @click="clickButton(index)">试驾</view>
				</view>
			</view>
		</view>

		<view class="button-buttom-test-drive" @click="clickButtonBottom">
			<view class="button-buttom-title">
				立即预约试驾
			</view>
			<view class="button-buttom-description">
				{{appointment}}人已预约
			</view>
		</view>
		<view>
			<u-popup v-model="showScore" mode="bottom" height="40%" border-radius="16">
				<view class="popup">
					<image class="popup-close" @click="closeScorePopup" src="/static/icon_close.png"></image>
					<view class="popup-title">
						您觉得本车如何
					</view>
					<view class="popup-explian">
						满分5分
					</view>
					<view class="popup-submit" @click="clickScoreSubmit">
						提交
					</view>
				</view>
			</u-popup>
		</view>

		<view>
			<u-popup v-model="showTestDrive" mode="bottom" height="40%" border-radius="16">
				<view class="popup">

					<image class="popup-close" @click="closeTestDrivePopup" src="/static/icon_close.png"></image>

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
					<view class="popup-submit" @click="clickTestDriveSubmit">
						提交
					</view>
				</view>
			</u-popup>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				seriesName: '',
				brandName: '奔驰',
				seriesTag: '中型车',
				carImageUrl: 'https://p3-dcd.byteimg.com/img/tos-cn-i-0000/7ac7c8a6aaff4e03a0e8cb997b88e37b~tplv-resize:1200:0.webp',
				minPrice: 18.89,
				maxPrice: 40.21,
				minGuidePrice: 30.78,
				maxGuidePrice: 47.78,
				rate: 3.58,
				tagList: ['在售', '2020款', '2019款', '2018款'],
				currentTag: 0,
				showCarList: [{
					year:2020,
					name: '30 TFSI 进取型',
					price: 20.11,
					guideprice: 47.78,
					id: '',
				}, {
					year:2019,
					name: '30 TFSI 运动型',
					price: 18.89,
					guideprice: 31.08,
					id: '',
				}, {
					year:2018,
					name: '30 TFSI 精英型',
					price: 22.51,
					guideprice: 31.08,
					id: '',
				}, ],
				allCarList:[{
					year:2020,
					name: '30 TFSI 进取型',
					price: 20.11,
					guideprice: 47.78,
					id: '',
				}, {
					year:2019,
					name: '30 TFSI 运动型',
					price: 18.89,
					guideprice: 31.08,
					id: '',
				}, {
					year:2018,
					name: '30 TFSI 精英型',
					price: 22.51,
					guideprice: 31.08,
					id: '',
				}, ],
				appointment: 2398,
				showScore: false,
				showTestDrive: false,
				testdrive: '',
				city: '',
				name: '',
				phone: '',
				stars: 3,

			}
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数
			console.log(option.id); //打印出上个页面传递的参数。
			console.log(option.name); //打印出上个页面传递的参数。
			if (option.name) {
				uni.setNavigationBarTitle({
					title: option.name
				});
				this.seriesName = option.name;
			}
		},
		methods: {
			clickTag(e) {
				console.log(e);
				if (this.currentTag != e) {
					this.currentTag = e;
					if(e==0){
						this.showCarList = this.allCarList;
					}else{
						let requestYear = this.tagList[e].slice(0,4);
						let showCarList = this.allCarList.filter(item=>{
							return item.year == requestYear;
						})
						this.showCarList = showCarList;
					}
				}
			},
			score(e) {
				this.showScore = true;
			},
			closeScorePopup(e) {
				this.showScore = false;
			},
			closeTestDrivePopup(e) {
				this.showTestDrive = false;
			},
			clickButton(e) {
				console.log(e);
				this.testdrive = e;
				this.showTestDrive = true;
			},
			clickButtonBottom(e) {
				this.testdrive = '';
				this.showTestDrive = true;
			},
			clickTestDriveSubmit(e) {
				console.log(e)
			},
			clickScoreSubmit(e) {
				console.log(e)
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
	.car-image {
		position: absolute;
		z-index: 100;
		width: 100%;
		height: 320rpx;
	}

	.background {
		position: relative;
		z-index: 99;
		width: 100%;
		height: 320rpx;
	}

	.flex-column {
		padding: 12rpx 28rpx;
		display: flex;
		flex-direction: column;
	}

	.title {
		font-family: PingFangSC-Medium;
		font-size: 40rpx;
		color: #1F2129;
		letter-spacing: 0;
		line-height: 56rpx;
		font-weight: 500;
	}

	.description {
		padding: 12rpx;
		font-family: PingFangSC-Regular;
		font-size: 22rpx;
		color: #979AA8;
		letter-spacing: 0;
		line-height: 36rpx;
		font-weight: 400;
	}

	.price {
		font-family: PingFangSC-Semibold;
		font-size: 34rpx;
		color: #E62021;
		letter-spacing: 0;
		line-height: 48rpx;
		font-weight: 600;
	}

	.price-guide {
		padding: 12rpx;
		font-family: PingFangSC-Regular;
		font-size: 22rpx;
		color: #1F2129;
		letter-spacing: 0;
		line-height: 36rpx;
		font-weight: 400;
	}

	.box-rate {
		margin-left: auto;
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

	.cars {
		display: flex;
		flex-direction: column;
		justify-content: flex-start;
	}

	.car-name {
		font-family: PingFangSC-Medium;
		font-size: 30rpx;
		color: #1F2129;
		letter-spacing: 0;
		line-height: 40rpx;
		font-weight: 500;
	}

	.car-price {
		font-family: PingFangSC-Semibold;
		font-size: 32rpx;
		color: #E62021;
		letter-spacing: 0;
		line-height: 40rpx;
		font-weight: 600;
	}

	.car-price-guide {
		padding: 12rpx;
		font-family: PingFangSC-Regular;
		font-size: 24rpx;
		color: #979AA8;
		letter-spacing: 0;
		line-height: 36rpx;
		font-weight: 400;
	}

	.button-test-drive {
		margin-left: auto;
		margin-right: 28rpx;
		width: 180rpx;
		height: 56rpx;
		background: rgba(255, 250, 233, 0.80);
		border: 0 solid #FFCD32;
		border-radius: 4rpx;
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		font-family: PingFangSC-Medium;
		font-size: 28rpx;
		color: #1F2129;
		letter-spacing: 0;
		text-align: center;
		line-height: 28rpx;
		font-weight: 500;
	}

	.button-buttom-test-drive {
		position: fixed;
		bottom: 68rpx;
		left: 32rpx;
		width: 686rpx;
		height: 88rpx;
		background: #FFCD32;
		border-radius: 4rpx;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	.button-buttom-title {
		font-family: PingFangSC-Medium;
		font-size: 32rpx;
		color: #1F2129;
		letter-spacing: 0;
		text-align: center;
		line-height: 32rpx;
		font-weight: 500;
	}

	.button-buttom-description {
		opacity: 0.6;
		font-family: PingFangSC-Regular;
		font-size: 20rpx;
		color: #000000;
		letter-spacing: 0;
		text-align: center;
		line-height: 20rpx;
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

	.popup-form {
		width: 686rpx;
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
