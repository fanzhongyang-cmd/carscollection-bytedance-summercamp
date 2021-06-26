<template>
	<view>
		<view class="tags">
			<view v-for="(tag,index) in tagList" :key="index" @click="clickTag(index)">
				<u-tag class="tag" :text="tag" mode="light" color="#000000"
					:bg-color="currentTag==index?'#FFFFFF':'#F7F8FC'"
					:border-color="currentTag==index?'#606370':'#F7F8FC'" shape="square" />
			</view>
		</view>
		
		<view class="content">
			<view v-for="(series,index) in showSeriesList">
				<view class="card-series">
					<image class="card-series-image" :src="series.image"></image>
					<view class="card-series-info">
						<view class="card-serires-name">{{series.name}}</view>
						<view class="card-serires-stars">综合:{{series.overallStars}}</view>
						<view class="card-serires-stars">我的:{{series.myStars}}</view>
					</view>
					<view class="card-serires-button" @click="reScore(index)">重新评分</view>
				</view>
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
	</view>
</template>

<script>
	export default {
		data(){
			return{
				showScore:false,
				currentTag: 0,
				tagList: ['全部','太差','一般','可以','不错','很棒'],
				showSeriesList: [{
					id:'123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马1系',
					overallStars: 3,
					myStars:4,
				}, {
					id:'123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马2系',
					overallStars: 3,
					myStars:4,
				}, {
					id:'123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马2系旅行车新能源(进口)',
					overallStars: 3,
					myStars:4,
				}, ],
				allSeriesList: [{
					id:'123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马1系',
					overallStars: 3,
					myStars:4,
				}, {
					id:'123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马2系',
					overallStars: 3,
					myStars:4,
				}, {
					id:'123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马2系旅行车新能源(进口)',
					overallStars: 3,
					myStars:4,
				}, {
					id:'123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马2系旅行车新能源(进口)',
					overallStars: 3,
					myStars:4,
				}],
			}
		},
		onLoad:function(options) {
			console.log(options)
		},
		methods:{
			clickTag(e) {
				console.log(e);
				if (this.currentTag != e) {
					this.currentTag = e;
					if(e==0){
						this.showSeriesList = this.allSeriesList;
					}else{
						let showSeriesList = this.allSeriesList.filter(item=>{
							return item.myStars == e;
						})
						this.showSeriesList = showSeriesList;
					}
				}
			},
			reScore(e) {
				this.showScore = true;
			},
			closeScorePopup(e) {
				this.showScore = false;
			},
			clickScoreSubmit(e) {
				console.log(e)
			},
		},
	}
</script>

<style>
	.tags {
		padding: 24rpx 28rpx;
		display: flex;
		flex-direction: row;
		justify-content: flex-start;
	}
	
	.tag {
		padding: 8rpx;
	}
	.content {
		width: 100%;
		display: flex;
		flex-direction: column;
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
