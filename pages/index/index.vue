<template>
	<view class="content">
		<!-- 头部轮播 -->
		<swiper class="swiper" circular :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration">
			<swiper-item v-for="(item,index) in banners" :key="index">
				<view class="swiper-item uni-bg-red">
					<img :src="item.path" alt="">
				</view>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	import swiper from './swiper.vue'
	export default {
		data() {
			return {
				title:'首页',
				banners: [],
				indicatorDots: true,
				autoplay: true,
				interval: 4000,
				duration: 500
			}
		},
		onLoad() {
			uni.request({
			    url: 'http://www.oldbaby.com/api',
				method: 'post',
			    success: (res) => {
			        this.banners = res.data.data.banners;
			    }
			});
		},
		methods: {
			
		},
		components: {
			swiper,
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
	.uni-margin-wrap {
		width:690rpx;
		width: 100%;;
	}
	.swiper {
		width:100%;
		height: 300rpx;
	}
	.swiper-item {
		display: block;
		height: 300rpx;
		line-height: 300rpx;
		text-align: center;
	}
	
	.swiper-list {
		margin-top: 40rpx;
		margin-bottom: 0;
	}
	
	.uni-common-mt{
		margin-top:60rpx;
		position:relative;
	}
	
	.info {
		position: absolute;
		right:20rpx;
	}
	
	.uni-padding-wrap {
	    width:550rpx;
	    padding:0 100rpx;
	}
</style>
