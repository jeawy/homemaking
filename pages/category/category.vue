<template>
  <view class="rf-category">
	  <!--顶部搜索导航栏-->
    <view class="nev">      
	  <view class="search">
		  <view class="search_position">
			  <view class="left_text">西安</view>
			  <image class="location_img" src="/static/home/City.svg"></image>
		  </view>
		  <view class="search_box">
			   <image class="search_img" src="/static/houseKeeping/search.svg"></image>   
			   <input class="search_input" placeholder="搜索关键字"/>
			   <image class="search_talk" src="/static/houseKeeping/microphone.svg"></image>  
		  </view>
		  <view class="notice">
		  		<image class="notice_img" src="/static/home/Notification.svg"></image>	  
		  </view>
	  </view>
    </view>
      <!-- 上侧分类导航 -->
      <!-- <scroll-view scroll-x="true" class="left"> -->
        <view class="top" >
			<view class="top_left">
				<view class="left_one" @tap="tabClick('recommend')"
				:class="{tab_active: currentIndex === 'recommend'}">推荐</view>
				<view class="left_two" @tap="tabClick('new')"
				:class="{tab_active: currentIndex === 'new'}">最新</view>
				<view class="left_three" @tap="tabClick('nearby')"
				:class="{tab_active: currentIndex === 'nearby'}">附近</view>
			</view>
			<view class="top_right">
				<view class="right_one" @tap="tabRightClick('screen')"
				:class="{tab_active: tabIndex === 'screen'}">筛选</view>
				<image class="right_img"  src="/static/houseKeeping/triangle.svg"></image>
				<view class="right_two" @tap="tabRightClick('classify')"
				:class="{tab_active: tabIndex === 'classify'}">分类</view>
				<image class="right_img" src="/static/houseKeeping/triangle.svg"></image>
			</view>			
        </view>
		<!-- <view class="line"></view> -->
      <!-- </scroll-view> -->
	  <!-- 推荐页面 -->
	  <view v-if="currentIndex == 'recommend'">
		  <scroll-view scroll-y="true">
			  <view  class="content">
				  <mainCard  class="main_card" :info="item" v-for="(item,index) in infolst_recommend" :key="index"/>
			  </view>
			
		  </scroll-view>
	  </view>
	  <!-- 最新页面-->
	  <view  v-if="currentIndex == 'new'">
		 <scroll-view scroll-y="true">
			 <view class="content">
				 <mainCard class="main_card" :info="item" v-for="(item,index) in infolst_new" :key="index"/>
			 </view>			
		 </scroll-view>
	  </view>
	  <!-- 附近页面-->
	  <view v-if="currentIndex == 'nearby'">		  
		  <scroll-view scroll-y="true">
			  <view class="nearby_map">
				<map/>
			  </view>
			  <view  class="content">
				  <mainCard class="main_card" :info="item" v-for="(item,index) in infolst_recommend" :key="index"/>
			  </view>
			
		   </scroll-view>
	  </view>
      <!-- 筛选弹框 -->
	  
  </view>
</template>
<script>
	import mainCard from '@/components/main-card.vue';
	/**
	 * @des 分类
	 *
	 * @author stav stavyan@qq.com
	 * @date 2020-03-10 18:19
	 * @copyright 2019
	 */
	import {productCate} from '@/api/product';
	import rfSearchBar from '@/components/rf-search-bar';
	import {advList} from '@/api/basic';
	export default {
		components: {
			rfSearchBar,
			mainCard
		},
		data() {
			return {
				// hotSearchDefault: '请输入关键字', // 搜索默认关键字
				// showCategoryIndex: 0, // 一级菜单高亮显示序号
				// categoryList: ['推荐','最近','附近'], // 左上角分类列表
				// search: {},
				// cateTop: {}, // 分类通用广告图
				// animation: 'animation-slide-right',
				// loading: true,
				// errorImage: this.$mAssetsPath.errorImage,
				currentIndex:'recommend',
				tabIndex:'',
				infolst_recommend:[{
					name:'张三',
					imgsrc:"/static/people.svg",
					age:27,
					sex:"women",
					position:'澳大利亚',
					time:"2",
					language:['普通话','英语'],
					type:"包月小时工"
					},{
						name:'爱丽丝',
						imgsrc:"/static/people.svg",
						age:27,
						sex:"women",
						position:'澳大利亚',
						time:"2",
						language:['普通话','英语'],
						type:"包月小时工"
					},{
						name:'张三',
						imgsrc:"/static/people.svg",
						age:27,
						sex:"women",
						position:'澳大利亚',
						time:"2",
						language:['普通话','英语'],
						type:"包月小时工"
					},{
						name:'爱丽丝',
						imgsrc:"/static/people.svg",
						age:27,
						sex:"women",
						position:'澳大利亚',
						time:"2",
						language:['普通话','英语'],
						type:"包月小时工"
					}],
				infolst_new:[{
					name:'marry',
					imgsrc:"/static/people.svg",
					age:27,
					sex:"women",
					position:'澳大利亚',
					time:"2",
					language:['普通话','英语'],
					type:"包月小时工"
					},{
						name:'汤姆',
						imgsrc:"/static/people.svg",
						age:27,
						sex:"women",
						position:'澳大利亚',
						time:"2",
						language:['普通话','英语'],
						type:"包月小时工"
					},{
						name:'张三',
						imgsrc:"/static/people.svg",
						age:27,
						sex:"women",
						position:'澳大利亚',
						time:"2",
						language:['普通话','英语'],
						type:"包月小时工"
					},{
						name:'爱丽丝',
						imgsrc:"/static/people.svg",
						age:27,
						sex:"women",
						position:'澳大利亚',
						time:"2",
						language:['普通话','英语'],
						type:"包月小时工"
					}],
			  }
		},
		// 每次页面显示 执行该方法
		onLoad(option) {
			this.initData();
			if(option.type == 1){
				this.currentIndex = 'recommend'
			}else{
				this.currentIndex = 'new'
			}
		},
		methods: {
			// 顶部tab点击
			tabClick(index){
				this.currentIndex = index;
			},
			tabRightClick(index){
				this.tabIndex = index;
			},
			// 跳转至商品列表
			navTo(route) {
			  this.$mRouter.push({route});
			},
			// 数据初始化
			async initData() {
				this.search = uni.getStorageSync('search') || {};
				this.hotSearchDefault = '请输入关键字 ' + (this.search.hot_search_default ? ('如: ' + this.search.hot_search_default) : '');
				this.getProductCate();
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
			// // 获取商品分类列表
			// async getProductCate() {
			// 	await this.$http.get(`${productCate}`).then(r => {
			// 		this.loading = false;
			// 		// 获取分类广告图 注：广告图不是一级分类图
   //        this.getAdvList();
   //        // 过滤掉没有二级菜单的一级菜单
			// 		r.data.forEach(item => {
   //          if (item.child.length > 0) {
   //          	this.categoryList.push(item)
   //          }
   //        });
			// 	}).catch(() => {
   //        this.loading = false;
   //      });
			// },
			// 获取广告列表
			async getAdvList() {
				await this.$http.get(`${advList}`, {
					location: 'cate_top'
				}).then(r => {
					this.cateTop = r.data.cate_top[0];
				});
			},
			//分类切换显示
			showCategory(index) {
				this.showCategoryIndex = index;
				this.animation= 'animation-slide-right';
				setTimeout(()=>{
					this.animation= '';
				}, 1000)
			},
			// 跳转至搜索详情页
			toSearch() {
			  this.$mRouter.push({route: `/pages/index/search/search?data=${JSON.stringify(this.search)}`});
			},
			// 跳转至分类页
			toIndex() {
        this.$mRouter.switchTab({route: '/pages/index/index'});
			},
			// 跳至广告图指定页面
			indexTopToDetailPage(type, url) {
				if (!type || !url) return;
				switch (type) {
					case 'product_view':
						url = `/pages/product/product?id=${url}`;
						break;
					case 'article_view':
						this.$mHelper.toast('article_view');
						break;
					case 'coupon_view':
						url = `/pages/user/coupon/detail?id=${url}`;
						break;
					case 'helper_view':
						this.$mHelper.toast('helper_view');
						break;
					case 'product_list_for_cate':
						url = `/pages/product/list?cate_id=${url}`;
						break;
					default:
						break;
				}
			  this.$mRouter.push({route: url});
			}
		}
	}
</script>
<style scoped lang="scss">
  page {
	  background-color: #FFFFFF;
  }
  .rf-category {
    background-color: #FFFFFF;
    .search,.search_position,.search_box,.top,.top_left,.top_right{
		  display: flex;
		  align-items: center;
    }
    .nev{
       padding-top: 51.48rpx;
    }
	.left_text,.search_input{
		font-family: Tensentype MingSongJ-W2;
		font-size: 12px;
		font-style: normal;
		font-weight: 400;
		line-height: 14px;
		letter-spacing: 0em;	
	}
    .search{     
      margin: 0 40rpx 10rpx 40rpx;
      height: 40rpx;
	  
      .search_position{
        
        .location_img{
		  margin-left: 4rpx;
          width: 16rpx;
          height: 24rpx;
        }
      }
      .search_box{
		  margin-left: 30rpx;
        width: 520rpx;
        background: rgba(118, 118, 128, 0.12);
        border-radius: 10px;
        .search_img{
          width: 22rpx;
          height: 22rpx;
          margin: 10rpx 12.2rpx 8rpx 12rpx;
        }
		.search_input{
			width: 446rpx;
		}
        .search_talk{
          width: 14.78rpx;
          height: 22rpx;
          margin: 10rpx 12.2rpx 8rpx 0;
        }
      }
      .notice{
        .notice_img{
          margin-left:30rpx;
          width: 22rpx;
          height: 21.54rpx;
        }
      }
    }
	  
	  .left_one,.left_two,.left_three,.right_one,.right_two{
	    font-family: Tensentype MingSongJ-W4;
	    font-size: 10px;
	    font-style: normal;
	    font-weight: 400;
	    line-height: 12px;
	    letter-spacing: 0em;
	    text-align: left;	  
	  }
    .top{
		 height: 32rpx;
		 margin: 10rpx 40rpx 8rpx 40rpx;
		 padding-bottom: 8rpx;
		 justify-content: space-between;
		 border-bottom: 2rpx solid rgba(255, 141, 14, 0.4);
		
		.top_left{
			.left_two{
			  padding: 0 20rpx;
			}			
		}
		.tab_active{
			color: #ff8d0e;
		}
		.top_right{
			.right_img{
				margin: 6rpx 20rpx 0 4rpx;
				width: 10rpx;
				height: 10rpx;
			}
		}     
    }
	.nearby_map{
		
		margin:10rpx 20rpx;
		map{
			width: 100%;
			height: 460rpx;
		}
	}
	.content{
		display: flex;
		width: 100%;
		margin-left: 20rpx;
		flex-wrap: wrap;
		.main_card{
			margin-right: 22rpx;
			margin-bottom: 20rpx;
		}
	}
	// .line{
	//   margin: 0 38rpx 0 40rpx;
	//   height: 2rpx;
	//   background: rgba(255, 141, 14, 0.4);
	//   transform: matrix(1, 0, 0, -1, 0, 0);
	// }
    /*模块分类*/
    .category-list {
      width: 100%;
      background-color: #fff;
      display: flex;

      .left, .right {
        position: absolute;
        top: 100upx;
        /*  #ifdef  APP-PLUS  */
        top: calc(100upx + var(--status-bar-height));
        /*  #endif  */
        bottom: 0upx;
      }

      .left {
        width: 24%;
        left: 0upx;
        background-color: #f2f2f2;

        .row {
          width: 100%;
          height: 90upx;
          display: flex;
          align-items: center;

          .text {
            width: 100%;
            position: relative;
            font-size: 28upx;
            display: flex;
            justify-content: center;
            color: #3c3c3c;

            .block {
              position: absolute;
              width: 0upx;
              left: 0;
            }
          }

          &.on {
            height: 100upx;
            background-color: #fff;

            .text {
              font-size: 30upx;
              font-weight: 600;
              color: #2d2d2d;

              .block {
                width: 10upx;
                height: 80%;
                top: 10%;
                background-color: #f06c7a;
              }
            }
          }
        }
      }

      .right {
        width: 76%;
        left: 24%;
        background-color: #fff;
        .category {
          width: calc(100%);
          padding: 20upx 15upx;

          .banner {
            width: 96%;
            margin: 0 auto 20upx;
            height: 24.262vw;
            border-radius: 10upx;
            overflow: hidden;
            box-shadow: 0upx 5upx 20upx rgba(0, 0, 0, 0.3);

            image {
              width: 100%;
              height: 24.262vw;
            }
          }

          .list {
            margin-top: 40upx;
            width: 100%;
            display: flex;
            flex-wrap: wrap;

            .box {
              width: calc(71.44vw / 3);
              margin-bottom: 30upx;
              display: flex;
              justify-content: center;
              align-items: center;
              flex-wrap: wrap;

              image {
                width: 64%;
                height: calc(71.44vw / 3 * 0.64);
              }

              .text {
                margin-top: 8upx;
                width: 100%;
                display: flex;
                justify-content: center;
                font-size: 26upx;
              }
            }
          }

          .no-data {
            margin: 48upx 0;
            color: $font-color-light;
            display: flex;
            justify-content: center;
            align-items: center;

            .iconfont {
              margin-right: 20upx;
              font-size: $font-lg + 16upx;
            }
          }
        }
      }
    }
  }
</style>
