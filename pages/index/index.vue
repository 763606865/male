<template>
	<view class="content">
		<!-- 头部轮播 -->
		<uni-swiper-dot class="uni-swiper" :info="banners" :current="current" field="content" :mode="mode">
		    <swiper class="banner-box" :autoplay="autoplay" :interval="interval" :duration="duration" circular @change="swiperChange">
		    	<swiper-item v-for="(item,index) in banners" :key="index">
		    		<view class="banner-item">
		    			<a :href="item.link"><img :src="cdn(item.path)" alt=""></a>
		    		</view>
		    	</swiper-item>
		    </swiper>
		</uni-swiper-dot>
		
		<tabBars></tabBars>
		</view>
</template>

<script>
	import tabBars from './components/tabBars.vue'
	import {uniSwiperDot}  from '@dcloudio/uni-ui'
	
	export default {
		data() {
			return {
				title:'首页',
				banners: [],
				indicatorDots: false,
				autoplay: true,
				interval: 4000,
				duration: 500,
				mode: 'round',
				current: 0,
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
			change(item) {
				uni.navigateTo({
					url: item.url,
				});
			},
			swiperChange(e) {
				this.current = e.detail.current;
			}
		},
		components:{
			tabBars,
			uniSwiperDot,
		}
	}
</script>

<style lang="scss">
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		
		.uni-swiper {
			width: 100%;
			.banner-box {
				width:100%;
				height: 500rpx;
				
				.banner-item {
					display: flex;
					height: 100%;
					text-align: center;
					
					img {
						width: 100%;
						height: 100%;
					}
				}
			}
		}
	}
</style>