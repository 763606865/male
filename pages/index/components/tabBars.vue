<template>
	<view class="tabs">
		<scroll-view id="tab-bar" class="scroll-h" :scroll-x="true" :show-scrollbar="false" :scroll-into-view="scrollInto">
			<view v-for="(tab,index) in tabBars" :key="tab.id" class="uni-tab-item" :id="tab.id" :data-current="index" @click="ontabtap">
				<text class="uni-tab-item-title" :class="tabIndex==index ? 'uni-tab-item-title-active' : ''">{{tab.name}}</text>
			</view>
		</scroll-view>
		<swiper :current="tabIndex" class="swiper-box" style="flex: 1;" :duration="300" @change="ontabchange">
			<swiper-item class="swiper-item">
				<follower></follower>
			</swiper-item>
			<swiper-item class="swiper-item">
				<recommend></recommend>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	import follower from './detail/follower.vue'
	import recommend from './detail/recommend.vue'
	// 缓存每页最多
	const MAX_CACHE_DATA = 100;
	// 缓存页签数量
	const MAX_CACHE_PAGE = 3;
	export default {
		data() {
			return {
				tabBars : [{
                    name: '关注',
                    id: 'follower'
                }, {
                    name: '推荐',
                    id: 'recommend'
                }],
				cacheTab: [],
				tabIndex: 0,
				scrollInto: "",
				showTips: false,
				navigateFlag: false,
				pulling: false,
			}
		},
		onLoad() {
			
		},
		methods: {
			goDetail(e) {
			    if (this.navigateFlag) {
			        return;
			    }
			    this.navigateFlag = true;
			    uni.navigateTo({
			        url: './detail/detail?title=' + e.title
			    });
			    setTimeout(() => {
			        this.navigateFlag = false;
			    }, 200)
			},
			close(index1, index2) {
			    uni.showModal({
			        content: '是否删除本条信息？',
			        success: (res) => {
			            if (res.confirm) {
			                this.newsList[index1].data.splice(index2, 1);
			            }
			        }
			    })
			},
			loadMore(e) {
			    setTimeout(() => {
			        this.getList(this.tabIndex);
			    }, 500)
			},
			ontabtap(e) {
			    let index = e.target.dataset.current || e.currentTarget.dataset.current;
			    this.switchTab(index);
			},
			ontabchange(e) {
			    let index = e.target.current || e.detail.current;
			    this.switchTab(index);
			},
			switchTab(index) {
			    if (this.tabIndex === index) {
			        return;
			    }
			
			    this.tabIndex = index;
			    this.scrollInto = this.tabBars[index].id;
			
			    // 释放 tabId
			    if (this.cacheTab.length > MAX_CACHE_PAGE) {
			        let cacheIndex = this.cacheTab[0];
			        this.clearTabData(cacheIndex);
			        this.cacheTab.splice(0, 1);
			        //console.log("remove cache index:: " + cacheIndex);
			    }
			},
			clearTabData(e) {
			    this.newsList[e].data.length = 0;
			    this.newsList[e].loadingText = "加载更多...";
			},
			refreshData() {},
			onrefresh(e) {
			    var tab = this.newsList[this.tabIndex];
			    if (!tab.refreshFlag) {
			        return;
			    }
			    tab.refreshing = true;
			    tab.refreshText = "正在刷新...";
			
			    setTimeout(() => {
			        this.refreshData();
			        this.pulling = true;
			        tab.refreshing = false;
					tab.refreshFlag = false;
			        tab.refreshText = "已刷新";
			        setTimeout(() => { // TODO fix ios和Android 动画时间相反问题
			            this.pulling = false;
			        }, 500);
			    }, 2000);
			},
			onpullingdown(e) {
			    var tab = this.newsList[this.tabIndex];
			    if (tab.refreshing || this.pulling) {
			        return;
			    }
			    if (Math.abs(e.pullingDistance) > Math.abs(e.viewHeight)) {
			        tab.refreshFlag = true;
			        tab.refreshText = "释放立即刷新";
			    } else {
			        tab.refreshFlag = false;
			        tab.refreshText = "下拉可以刷新";
			    }
			},
			newGuid() {
			    let s4 = function() {
			        return (65536 * (1 + Math.random()) | 0).toString(16).substring(1);
			    }
			    return (s4() + s4() + "-" + s4() + "-4" + s4().substr(0, 3) + "-" + s4() + "-" + s4() + s4() + s4()).toUpperCase();
			}
		},
		components:{
			follower,recommend
		}
	}
</script>

<style>
	.tabs {
	    flex: 1;
	    flex-direction: column;
	    overflow: hidden;
	    background-color: #ffffff;
	    height: 100vh;
		width:100%;
	}
	
	.scroll-h {
	    width: 750rpx;
		/* #ifdef H5 */
		width:100%;
		/* #endif */
		height: 80rpx;
	    flex-direction: row;
	    /* flex-wrap: nowrap; */
		align-content: flex-start;
	}
	
	.line-h {
	    height: 1rpx;
	    background-color: #cccccc;
	}
	
	.uni-tab-item {
	    /* #ifndef APP-PLUS */
	    display: inline-block;
	    /* #endif */
	    flex-wrap: nowrap;
	    padding-left: 34rpx;
	    padding-right: 34rpx;
		
	}
	
	.uni-tab-item-title {
	    color: #555;
	    font-size: 30rpx;
	    height: 80rpx;
	    line-height: 80rpx;
	    flex-wrap: nowrap;
	    /* #ifndef APP-PLUS */
	    white-space: nowrap;
	    /* #endif */
	}
	
	.uni-tab-item-title-active {
		font-size: 35rpx;
		font-weight: 700;
	}
	
	.swiper-box {
	    flex: 1;
	}
	
	.swiper-item {
	    flex: 1;
	    flex-direction: row;
	}
	
	.scroll-v {
	    flex: 1;
	    /* #ifndef MP-ALIPAY */
	    flex-direction: column;
	    /* #endif */
	    width: 750rpx;
		width:100%;
	}
	
	.update-tips {
	    position: absolute;
	    left: 0;
	    top: 41px;
	    right: 0;
	    padding-top: 5px;
	    padding-bottom: 5px;
	    background-color: #FDDD9B;
	    align-items: center;
	    justify-content: center;
	    text-align: center;
	}
	
	.update-tips-text {
	    font-size: 14px;
	    color: #ffffff;
	}
	
	.refresh {
	    width: 750rpx;
		width:100%;
	    height: 64px;
	    justify-content: center;
	}
	
	.refresh-view {
	    flex-direction: row;
	    flex-wrap: nowrap;
	    align-items: center;
	    justify-content: center;
	}
	
	.refresh-icon {
		width: 30px;
		height: 30px;
		transition-duration: .5s;
		transition-property: transform;
		transform: rotate(0deg);
		transform-origin: 15px 15px;
	}
	
	.refresh-icon-active {
		transform: rotate(180deg);
	}
	
	.loading-icon {
		width: 20px;
		height: 20px;
		margin-right: 5px;
		color: #999999;
	}
	
	.loading-text {
	    margin-left: 2px;
	    font-size: 16px;
	    color: #999999;
	}
	
	.loading-more {
	    align-items: center;
	    justify-content: center;
	    padding-top: 10px;
	    padding-bottom: 10px;
	    text-align: center;
	}
	
	.loading-more-text {
	    font-size: 28rpx;
	    color: #999;
	}
</style>
