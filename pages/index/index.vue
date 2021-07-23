<template>
	<view class="content">
		<!-- 头部轮播 -->
		<swiper class="banner-box" circular :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration">
			<swiper-item v-for="(item,index) in banners" :key="index">
				<view class="banner-item">
					<a :href="item.link"><img :src="item.path" alt=""></a>
				</view>
			</swiper-item>
		</swiper>
		
		<tabBars :stores="stores"></tabBars>
		</view>
</template>

<script>
	import tabBars from './components/tabBars.vue'
	export default {
		data() {
			return {
				title:'首页',
				banners: [],
				indicatorDots: false,
				autoplay: true,
				interval: 4000,
				duration: 500,
				stores:[],
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
			uni.request({
			    url: 'http://www.oldbaby.com/api/stores',
				method: 'post',
			    success: (res) => {
			        let data = res.data.data.stores.data;
					for(var i=0; i<data.length; i++)
					{
						data[i].mark = this.cdn(data[i].mark);
					}
					this.stores = data;
			    },
			});
		},
		methods: {
			change(item) {
				uni.navigateTo({
					url: item.url,
				});
			},
		},
		components:{
			tabBars,
		}
	}
</script>

<style lang="scss">
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		
		.banner-box {
			width:100%;
			height: 300rpx;
			
			.banner-item {
				display: flex;
				height: 300rpx;
				line-height: 300rpx;
				text-align: center;
				
				img {
					width: 100%;
					height: 100%;
				}
			}
		}
	}
</style>