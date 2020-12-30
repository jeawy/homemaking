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
			<view class="swiper-box">
				<!-- <rf-swipe-dot :info="carouselList.index_top" mode="nav" :current="current" field="title"> -->
					<swiper @change="handleDotChange" autoplay="true">
						<!-- <swiper-item
								v-for="(item, index) in carouselList.index_top"
								@tap="indexTopToDetailPage(item.jump_type, item.jump_link)"
								:key="index"> -->
						<swiper-item>
							<view class="swiper-item">
								<!-- <image :src="item.cover" mode="aspectFill"/> -->
								<image src="../../static/home/Swiper.svg" mode="aspectFit"/>
							</view>
						</swiper-item>
					</swiper>
				<!-- </rf-swipe-dot> -->
			</view>
		</view>
		<view class="swiper_dot" style="height: 20px;">
		</view>
		<!-- 分类列表 -->
		<view class="category-list">
			<view
				class="category"
				v-for="(item, index) in serveCateList"
				:key="index"
			>
				<image :src="item.src" mode="aspectFill"></image>
			</view>
		</view>
		<!--广告-->
		<view class="ads">
			<image class="ads_img" src="../../static/home/LeftAdsDiscount.svg"></image>
			<image class="ads_img" src="../../static/home/RightAdsLocal.svg"></image>
		</view>
		<!--VIP-->
		<view class="VIP">
			<image src="../../static/home/MidAdsVip.svg"></image>
		</view>
		<!--分享礼品-->
		<view class="gift_ads">
			<image class="gift_img" src="../../static/home/LastLeftShare.svg"></image>
			<image class="gift_img" src="../../static/home/LastRightReturn.svg"></image>
		</view>
	</view>
</template>
<script>
    /**
     * @des 微商城首页
     *
     * @author stav stavyan@qq.com
     * @date 2020-01-08 14:14
     * @copyright 2019
     */
    import {indexList} from '@/api/product';
    import rfSwipeDot from '@/components/rf-swipe-dot';
    import rfFloorIndex from '@/components/rf-floor-index';
    import rfSearchBar from '@/components/rf-search-bar';
    import rfSwiperSlide from '@/components/rf-swiper-slide';
    import {notifyAnnounceIndex} from '@/api/basic';
    import rfCountDown from '@/components/rf-count-down';

    export default {
        components: {rfFloorIndex, rfSwipeDot, rfSearchBar, rfSwiperSlide, rfCountDown},
        data() {
            return {
                current: 0, // 轮播图index
                carouselList: {}, // 广告图
                hotProductList: [], // 热门商品列表
                recommendProductList: [], // 推荐商品列表
                guessYouLikeProductList: [], // 猜你喜欢商品列表
                newProductList: [], // 新品上市商品列表
                serveCateList: [// 服务项目列表
					{src:"../../static/home/maternity.svg"},
					{src:"../../static/home/babymeal.svg"},
					{src:"../../static/home/kids.svg"},
					{src:"../../static/home/elderly.svg"},
					{src:"../../static/home/housekeeping.svg"},
					{src:"../../static/home/hoursworker.svg"},
					{src:"../../static/home/repairworker.svg"},
					{src:"../../static/home/painworker.svg"},
					{src:"../../static/home/moveworker.svg"},
					{src:"../../static/home/lawnmaintainance.svg"},
					],  
                config: {}, // 商户配置
                announceList: [], // 公告列表
                loading: true,
                hotSearchDefault: '请输入关键字',
                newsBg: this.$mAssetsPath.newsBg,
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
                this.current = e.detail.current
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
	page{
		background-color: #FFFFFF;
	}
	.rf-index {
		margin-top: 30px;
		// background-color: #FFFFFF;
		.header,.location{
			display: flex;
			align-items: center;
		}
		.header{
			justify-content: space-between;
		}
		// 定位
		.location{	
			// margin-top:56px;
			.location_text{
				margin-left: 20px;
				font-family: Tensentype MingSongJ-W2;
				font-style: Regular;
				font-size: 12px;
			}
			.location_img{
				padding-left: 2px;
				width: 8px;
				height: 12px;
			}
		}
		.notice{
			// margin-top:56px;
			.notice_img{
				margin-right: 19px;
				width: 11px;
				height: 10.77px;
			}
		}
		/*轮播图*/
		.swiper {
			margin: 10px 20px 0 20px;
			// height: 224px;
			.swiper-box {
				// width: 92%;
				// height: 40vw;
				// overflow: hidden;
				// border-radius: 15upx;
				// box-shadow: 0upx 8upx 25upx rgba(0, 0, 0, 0.2);
				// //兼容ios，微信小程序
				// position: relative;
				// z-index: 1;
		
				swiper {					
					width: 100%;
					height: 200px;
					swiper-item {
						border-radius: 9px;
						image {
							width: 335px;
							height: 200px;
							
						}
					}
				}
			}
		}
		/*分类列表*/
		.category-list {
			height: 150px;
			margin: 0 20px ;
			display: flex;
			flex-wrap: wrap;
			background-color: #ffe8cd;
			border-radius: 9px;
			.category {
				display: flex;
				flex-wrap: wrap;
					image {
						margin: 8px 7px;
						width: 50px;
						height: 54px;
					}
			}
		}
		.ads{
			margin: 10px 20px;
			display: flex;
			.ads_img{
				// width: 162.5px;
				height: 90px;
			}
		}
		.VIP{	
			margin: 0px 20px;
			height: 70px;
			image{
				position: relative;
				height: 70px;								
			}
		}
		.gift_ads{
			margin: 10px 20px 13px 20px;
			display: flex;
			.gift_img{
				// width: 162.5px;
				height: 70px;
			}
		}
		
	}
</style>
