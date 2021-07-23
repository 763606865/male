<template>	
	<view class="box">
		<view class="box-item" v-for="(item,index) in stores" :key="item.id" @tap="redirectDetail(item.id)">
			<!-- TODO 在支付宝小程序下 需要用 style 覆盖标签的默认样式 -->
			<view class="box-image">
				<image :src="item.mark" mode="aspectFit"></image>
			</view>
			<view class="box-title">
				<text style="flex-wrap: wrap;">{{item.title}}</text>
			</view>
			<view class="box-sale" style="display: flex; flex-direction: row; justify-content: space-between;">
				<text style="color: #DD524D; font-size: 30rpx;margin-right: 10rpx;">{{item.price}}</text>
				<text style="color: #b9c0bb; font-size: 24rpx;text-decoration: line-through; margin-right: 40rpx;">{{item.origin_price}}</text>
				<text style="color: #b9c0bb; font-size: 20rpx;">已售{{item.sales}}</text>
			</view>
			<view class="box-tags">
				<view class="box-tags-item" v-for="(tag,tag_index) in item.tags" :key="tag.id">
					<button size="mini" type="primary" plain hover-class="none">{{tag.name}}</button>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
			};
		},
		onLoad() {
		},
		computed: {
			
		},
		methods: {
			redirectDetail:function(store_id)
			{
				uni.navigateTo({
				    url: '/pages/store/detail?store_id='+store_id
				});
			}
		},
		props: {
			stores:{
				
			}
		}
	}
</script>
<style lang="scss">
	.box {
		display: flex;
		flex-direction: row;
		padding:10rpx;
		
		.box-item {
			flex-flow: column;
			padding: 10rpx;
			background: white;
			border-radius: 10rpx;
			width:50%;
			
			.box-image {
				width:100%;
				
				image {
					width: 100%;
				}
			}
			
			.box-title {
				font-size: 28rpx;
				height:200rpx;
				overflow: hidden;
				text-overflow:ellipsis;
			}
			.box-sale {
				margin:10rpx 0;
			}
			.box-tags {
				display: flex;
				flex-direction: row;
				flex-wrap:nowrap;
				
				.box-tags-item {
					margin: 0 10rpx;
					
					button {
						padding: 0 10rpx;
						line-height: 30rpx;
					}
				}
			}
		}
		
		.box-item:nth-child(1) {
			margin-right: 10rpx;
		}
	}
	
	
</style>