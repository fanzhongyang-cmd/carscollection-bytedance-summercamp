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
						<view class="card-serires-stars">{{series.stars}}</view>
						<view class="card-serires-price">指导价：{{series.minPrice}}万起</view>
					</view>
					<view class="card-serires-button">{{series.tag}}</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				currentTag: 0,
				tagList: ['全部', '已提交', '待试驾', '已试驾'],
				showSeriesList: [{
					id: '123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马1系',
					stars: 3,
					minPrice: 28.46,
					tag: '已提交',

				}, {
					id: '123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马2系',
					stars: 3,
					minPrice: 28.46,
					tag: '待试驾',

				}, {
					id: '123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马2系旅行车新能源(进口)',
					stars: 3,
					minPrice: 28.46,
					tag: '已试驾',

				}, ],
				allSeriesList: [{
					id: '123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马1系',
					stars: 3,
					minPrice: 28.46,
					tag: '已提交',

				}, {
					id: '123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马2系',
					stars: 3,
					minPrice: 28.46,
					tag: '待试驾',

				}, {
					id: '123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马2系旅行车新能源(进口)',
					stars: 3,
					minPrice: 28.46,
					tag: '已试驾',

				}, {
					id: '123456',
					image: 'https://sf1-ttcdn-tos.pstatp.com/img/motor-img/17a74f4d2df126624068905e612030b4~400x266.webp',
					name: '宝马2系旅行车新能源(进口)',
					stars: 3,
					minPrice: 88.88,
					tag: '已试驾',

				}],
			}
		},
		onLoad: function(options) {
			console.log(options)
		},
		methods: {
			clickTag(e) {
				console.log(e);
				if (this.currentTag != e) {
					this.currentTag = e;
					if(e==0){
						this.showSeriesList = this.allSeriesList;
					}else{
						let requestTag = this.tagList[e];
						let showSeriesList = this.allSeriesList.filter(item=>{
							return item.tag==requestTag;
						})
						this.showSeriesList = showSeriesList;
					}
					
				}
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
</style>
