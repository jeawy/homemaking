<template>
	<view class="rf-index">
		<!-- 顶部信息 -->
		<view class="header">
			<!--位置信息-->
			<view class="location">
				<view class="location_text">西安</view>
				<view>
					<image class="location_img" src="../../static/home/City.svg"></image>
				</view>				
			</view>
			<view class="notice">
				<image class="notice_img" src="../../static/home/Notification.svg"></image>
			</view>
		</view>	 
		<!-- 轮播图 -->
		<view class="swiper">
			<swiper @change="handleDotChange" :Lautoplay="true" :current="swiperCurrent" :circular='true'>
				<swiper-item v-for="(item,index) in swiperImg" :key="index">
					<view class="swiper-item">
						<image :src="item" mode="aspectFit"/>
					</view>
				</swiper-item>					
			</swiper>
			<!-- 轮播指示点样式修改 -->
			<view class="dots">
				<block v-for="(item,index) in swiperImg.length" :key="item">
				  <view class="dot" :class="index==swiperCurrent ? ' active' : ''"></view>
				</block>
			</view>
		</view>
		<!-- 分类列表 -->
		<view class="category-list">
			<view class="cate">
				<image class="cate_image_one" src="../../static/home/maternity.svg" mode="aspectFill"></image>
				<image class="cate_image_two" src="../../static/home/babymeal.svg" mode="aspectFill"></image>
				<image class="cate_image_three" src="../../static/home/kids.svg" mode="aspectFill"></image>
				<image class="cate_image_four" src="../../static/home/elderly.svg" mode="aspectFill"></image>
				<image class="cate_image_five" src="../../static/home/housekeeping.svg" mode="aspectFill"></image>
				<image class="cate_image_six" src="../../static/home/hoursworker.svg" mode="aspectFill"></image>
				<image class="cate_image_seven" src="../../static/home/repairworker.svg" mode="aspectFill"></image>
				<image class="cate_image_eight" src="../../static/home/painworker.svg" mode="aspectFill"></image>
				<image class="cate_image_nine" src="../../static/home/moveworker.svg" mode="aspectFill"></image>
				<image class="cate_image_ten" src="../../static/home/lawnmaintainance.svg" mode="aspectFill"></image>
			</view>		
		</view>
		
		<view class="middle">
			<!--广告-->
			<view class="ads">
				<image class="ads_left_img" src="../../static/home/LeftAdsDiscount.svg"></image>
				<image class="ads_img" src="../../static/home/RightAdsLocal.svg"></image>
			</view>
			<!--VIP-->
			<image class="vip_img" src="../../static/home/MidAdsVip.svg"></image>
		</view>
		<!--分享礼品-->
		<view class="gift_ads">
			<view class="gift">
				<image class="gift_left_img" src="../../static/home/LastLeftShare.svg"></image>
				<image class="gift_img" src="../../static/home/LastRightReturn.svg"></image>
			</view>			
		</view>
	</view>
</template>
<script>
    export default {
        data() {
            return {
				swiperImg: [// 轮播图图片
					'../../static/home/Swiper.svg',
					'../../static/home/Swiper.svg',
					'../../static/home/Swiper.svg',
				],
				swiperCurrent: 0, // 轮播图index 
                // config: {}, // 商户配置
                // announceList: [], // 公告列表
                // loading: true,
                // hotSearchDefault: '请输入关键字',
                // newsBg: this.$mAssetsPath.newsBg,
                // errorImage: this.$mAssetsPath.errorImage
            };
        },
        onShow() {
            this.initData();
        },
        computed: {
            // 计算倒计时时间
            second() {
                return function (val) {
                    return Math.floor(15 * 60 - (new Date() / 1000 - val))
                }
            }
        },
        onShareAppMessage() {
            return {
                title: '欢迎来到XXX商城',
                path: '/pages/index/index'
            }
        },
        filters: {
            filterDiscountPrice(val) {
                const price = val.product && val.product.price * val.discount / 100;
                switch (val.decimal_reservation_number) {
                    case 0:
                        return (Math.floor(price * 100) / 100).toFixed(2);
                    case 1:
                        return (Math.floor(price * 100) / 100).toFixed(0);
                    case 2:
                        return (Math.floor(price * 100) / 100).toFixed(1);
                    default:
                        return (Math.floor(price * 100) / 100).toFixed(2);
                }
            }
        },
        //下拉刷新
        onPullDownRefresh() {
            this.getIndexList('refresh');
        },
        methods: {
            // 监听轮播图切换
            handleDotChange(e) {
				this.swiperCurrent = e.detail.current;
            },
            // 数据初始化
            initData() {
              // 设置购物车数量角标
              this.getIndexList();
              this.initCartItemCount();
            },
            // 设置购物车数量角标
            async initCartItemCount() {
							if (this.$mStore.getters.hasLogin && parseInt(uni.getStorageSync('cartNum'), 10) > 0) {
                await uni.setTabBarBadge({
                  index: this.$mConstDataConfig.cartIndex,
                  text: uni.getStorageSync('cartNum').toString()
                });
							} else {
                uni.removeStorageSync('cartNum');
                uni.removeTabBarBadge({index: this.$mConstDataConfig.cartIndex});
							}
            },
            // 通用跳转
            navTo(route) {
                this.$mRouter.push({route});
            },
            // 通用跳转
            navToSearch() {
                this.$mRouter.push({route: `/pages/index/search/search?data=${JSON.stringify(this.search)}`});
            },
            // 跳至广告图指定页面
            indexTopToDetailPage(data, link) {
                if (isNaN(parseInt(data.id, 10))) {
                    // #ifdef  APP-PLUS
		                window.location.href = data.id;
										// #endif
                }
                let url;
                let type;
                let id;
                if (!link) {
                    type = data.type;
                    id = data.id;
                } else {
                    id = link;
                    type = data;
                }
                switch (type) {
                    case 'product_view':  // 产品详情
                        url = `/pages/product/product?id=${id}`;
                        break;
                    case 'article_view':  // 文章详情
                        this.$mHelper.toast('article_view');
                        break;
                    case 'coupon_view': // 优惠券详情
                        url = `/pages/user/coupon/detail?id=${id}`;
                        break;
                    case 'helper_view': //   站点帮助详情
                        this.$mHelper.toast('helper_view');
                        break;
                    case 'product_list_for_cate': // 某分类下产品列表
                        url = `/pages/product/list?cate_id=${id}`;
                        break;
                    default:
                        break;
                }
                if (url) {
			              this.$mRouter.push({route: url});
                }
            },
            // 获取主页数据
            async getIndexList(type) {
                await this.$http.get(`${indexList}`, {}).then(async r => {
                    this.loading = false;
                    if (type === 'refresh') {
                        uni.stopPullDownRefresh();
                    }
                    // 获取公告列表
                    await this.getNotifyAnnounceIndex();
                    // 首页参数赋值
                    this.initIndexData(r.data);
                }).catch(() => {
                    this.loading = false;
                    if (type === 'refresh') {
                        uni.stopPullDownRefresh();
                    }
                })
            },
            // 首页参数赋值
            initIndexData(data) {
                this.productCateList = data.cate;
                this.carouselList = data.adv;
                this.search = data.search;
                uni.setStorageSync('search', this.search);
                this.hotSearchDefault = '请输入关键字' + (data.search.hot_search_default ? `如: ${data.search.hot_search_default}` : '');
                uni.setStorage({
                    key: 'hotSearchDefault',
                    data: data.search.hot_search_default
                });
                this.hotProductList = data.product_hot;
                this.recommendProductList = data.product_recommend;
                this.guessYouLikeProductList = data.guess_you_like;
                this.newProductList = data.product_new;
                this.config = data.config;
            },
            // 获取通知列表
            async getNotifyAnnounceIndex() {
                await this.$http.get(`${notifyAnnounceIndex}`).then(r => {
                    this.announceList = r.data
                })
            },
            // 跳转至商品详情页
            navToDetailPage(data) {
                const {id} = data;
                if (!id) return;
                this.$mRouter.push({route: `/pages/product/product?id=${id}`});
            },
            // 跳转至分类页
            toCategory() {
                this.$mRouter.switchTab({route: '/pages/category/category'});
            }
        }
    }
</script>

<style lang="scss" scoped>
	
	.rf-index {
		background-color: #FFFFFF;
		width: 100%;		
		.header,.location{
			display: flex;
			align-items: center;
		}
		.header{
			padding-top: 56rpx;
			justify-content: space-between;
		}
		// 定位
		.location{	
			.location_text{
				margin-left: 40rpx;
				font-family: Tensentype MingSongJ-W2;
				font-style: Regular;
				font-size: 24rpx;
			}
			.location_img{
				padding-left: 4rpx;
				width: 16rpx;
				height: 24rpx;
			}
		}
		.notice{
			.notice_img{
				margin-right: 38rpx;
				width: 22rpx;
				height: 21.54rpx;
			}
		}
		/*轮播图*/		
		.swiper {
			padding-top: 20rpx;
			margin: 0 40rpx 0 40rpx;
			height: 468rpx;		
			swiper {					
				width: 100%;
				height: 400rpx;
				swiper-item {
					border-radius: 18rpx;
					image {
						width: 670rpx;
						height: 400rpx;
						
					}
				}
			}
		}
		.dots {
			  margin: 20rpx auto;
			  display: flex;
			  flex-direction: row;
			  justify-content: center;

			  .dot {
				  width: 22rpx;
				  height: 8rpx;
				  background: #ffcb98;
				  border-radius: 10rpx;
				  margin-right: 4rpx;
			  }
			  .active {
				  width: 50rpx;
				  height: 8rpx;
				  background: #FF8000;
				  border-radius: 10rpx;

			  }
		  }
		/*分类列表*/
		.category-list {
			height: 300rpx;
			margin: 0 40rpx;
			display: flex;
			flex-wrap: wrap;
			justify-content: space-around;
			background-color: #FFE8CD;
			border-radius: 16rpx;
			box-shadow: 0 8rpx 0 #d9d9d9;
			.cate{
				padding: 28rpx 36rpx 30rpx 38rpx;
				width: 100%;
				height: 300rpx;
				display: flex;
				flex-wrap: wrap;
					.cate_image_one{
						width: 100rpx;
						height: 102rpx;
					}
					.cate_image_two,
					.cate_image_three,.cate_image_four,
					.cate_image_five{
						width: 100rpx;
						height: 102rpx;
						margin-left: 24rpx;
					}
					.cate_image_seven,
					.cate_image_eight,.cate_image_nine,
					.cate_image_ten{
						width: 100rpx;
						height: 108rpx;
						margin-top: 32rpx;
						margin-left: 24rpx;
					}
					.cate_image_six{
						width: 100rpx;
						height: 108rpx;
						margin-top: 32rpx;
					}
					.cate_image_nine{
						height: 110rpx;
					}
			}
		}
		.middle{
			height: 357.68rpx;
			margin-top: 20rpx;
			.ads{
				display: flex;
				margin-right: 12rpx;
				height: 180rpx;
				.ads_left_img{
					position: relative;
					left: 18rpx;
					height: 190rpx;
				}
				.ads_img{
					height: 190rpx;
					position: relative;
					right: 10rpx;
				}
			}
			.vip_img{
				position: relative;
				top: -16rpx;
				height: 200rpx;
				width: 100%;
			}
		}
		.gift_ads{
			height: 158rpx;
			.gift{
				display: flex;
				height: 140rpx;
				.gift_left_img{
					position: relative;
					left: 18rpx;
					height: 148rpx;
				}
				.gift_img{
					position: relative;
					right: 18rpx;
					height: 148rpx;
				}
			}
		}		
	}
</style>
