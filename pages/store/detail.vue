<template>
	<view>
		<swiper class="albums" :indicator-dots="indicatorDots" :autoplay="autoplay">
			<swiper-item v-for="(item,index) in albums" :key="index">
				<view>
					<image :src="cdn(item.thumb)" mode="aspectFill"></image>
				</view>
			</swiper-item>
		</swiper>
		<view class="detail-sales">
			<view class="detail-price">
				<text style="font-size: 38rpx;margin-right: 20rpx;">¥{{info.price}}</text>
				<text style="font-size: 24rpx;text-decoration: line-through;">¥{{info.origin_price}}</text>
			</view>
			<view class="detail-sale">
				<text space="ensp">已售: {{info.sales}}</text>
				<text space="ensp"><uni-icons type="eye" size="10" space="ensp"></uni-icons>浏览: {{info.views}}</text>
			</view>
		</view>
		<view class="detail-box">
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title:'详情',
				indicatorDots: true,
				autoplay: false,
				info:[],
				platforms:[],
				albums:[],
				store_id:0,
			}
		},
		onLoad:function(option) {
			this.store_id = option.store_id;
			uni.request({
			    url: 'http://www.oldbaby.com/api/stores/'+this.store_id,
				method: 'post',
			    success: (res) => {
			        this.info = res.data.data.info;
			        this.platforms = res.data.data.platforms;
			        this.albums = res.data.data.albums;
					wx.setNavigationBarTitle({
					   title: this.info.name
					})
			    }
			});
		},
		methods: {
			
		},
		components:{}
	}
</script>

<style lang="scss">
	.albums {
		height:500rpx;
		image {
			width:100%;
		}
	}
	.detail-sales {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
		height:100rpx;
		
		.detail-price {
			width:70%;
			height:100%;
			line-height: 100rpx;
			flex-direction: row;
			justify-content: flex-start;
			padding-left: 20rpx;
			color: white;
			background: url('~@/static/images/price-background.jpeg');
		}
		.detail-sale {
			width: 30%;
			height: 100;
			text-align: center;
			font-size: 24rpx;
			line-height: 50rpx;
			background: #fbdae1;
			flex-direction: column;
			display: flex;
		}
	}

	.detail-box {
		padding:10rpx;
		background-color: #eef2ff;
	}
</style>
