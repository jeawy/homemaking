<template>
	<view class="rf-index">
		<!-- 顶部信息 -->
		<view class="header">
			<citySearch :city="city" @doGetLocation="doGetLocation"/>
			<view class="notice" @tap="navTo('/pages/user/notice/notice')">
				<image class="notice_img" src="../../static/home/Notification.svg"></image>
			</view>
		</view>	
		 <scroll-view
		 	class="main_content"
		 	scroll-y
		 >
			<!-- 轮播图 -->
			<view class="swiper">
				<swiper @change="handleDotChange" :Lautoplay="true" :current="swiperCurrent" :circular='true'>
					<swiper-item v-for="(item,index) in swiperImg" :key="index">
						<view class="swiper-item">
							<image :src="item" mode="aspectFill"/>
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
			<categoryLists @target="target" />
			<view class="middle">
				<!--广告-->
				<view class="ads">
					<image class="ads_left_img" src="../../static/home/LeftAdsDiscount.png"></image>
					<image class="ads_img" src="../../static/home/RightAdsLocal.png"></image>
				</view>
				<!--VIP-->
				<view class="vip">
					<image class="vip_img" src="../../static/home/MidAdsVip.png"></image>
					<image class="vip_img_top" src="../../static/home/Group.png"></image>
				</view>
			
			</view>
			<!--分享礼品-->
			<view class="gift_ads">
				<view class="gift">
					<image class="gift_left_img" src="../../static/home/LastLeftShare.png"></image>
					<image class="gift_img" src="../../static/home/LastRightReturn.png"></image>
				</view>			
			</view>
			<!-- 阿姨推荐 -->
			<view class="aunt_bottom" >
				<view class="aunt_top"><text class="bottom_text">阿姨推荐</text></view>
				<view class="bottom" >
					<mainCard class="main_card" :info="item" v-for="(item,index) in infolst" :key="index"/>
				</view>				
			</view>	
			<view class="search" @tap="target('/pages/category/category')">
				<view class="search_more">查看更多</view>
			</view>
			
			<!-- 新人推荐 -->
			<view class="new_bottom" >
				<view class="new_top"><text class="bottom_text">新人推荐</text></view>
				<view class="bottom" >
					<mainCard class="main_card" :info="item" v-for="(item,index) in newbee_infolst" :key="index"/>
				</view>
			</view>	
			<view class="search" @tap="target('/pages/category/category')">
				<view class="search_more">查看更多</view>
			</view>
			<view class="bottom_assure">
				<view class="assure_one">
					<view class="assure_text">保证服务质量</view>
					<image style="width: 28rpx;
						height: 28rpx;" class="bottom_img" src="../../static/home/correct.svg"></image>
				</view>
				<view class="assure_one">
					<view class="assure_text">无接触面试</view>
					<image style="width: 28rpx;
						height: 28rpx;" class="bottom_img" src="../../static/home/correct.svg"></image>
				</view>
				<view class="assure_one">
					<view class="assure_text">未服务全额退</view>
					<image style="width: 28rpx;
						height: 28rpx;" class="bottom_img" src="../../static/home/correct.svg"></image>
				</view>
			</view>
		</scroll-view>
	</view>
</template>
<script>
	import { speadList } from '@/api/spread';
	import {categoryList} from '@/api/product';
	import mainCard from '@/components/main-card.vue';
	import sPopup from '@/components/s-popup/index.vue';
	import citySearch from '@/components/city-search.vue';
	import categoryLists from '@/components/category-lists.vue'
    export default {
        data() {
            return {
				city:'',//当前所在的城市名称
				//cityVisiable:false,
				swiperImg: [// 轮播图图片
					'../../static/home/Swiper.svg',
				],
				baseurl:"",
				swiperCurrent: 0, // 轮播图index 
				infolst:[{
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
				newbee_infolst:[]

                // config: {}, // 商户配置
                // announceList: [], // 公告列表
                // loading: true,
                // hotSearchDefault: '请输入关键字',
                // newsBg: this.$mAssetsPath.newsBg,
                // errorImage: this.$mAssetsPath.errorImage
            };
        },
        components: {
        	mainCard,
			sPopup,
			citySearch,
			categoryLists
        },
		onShow() {
            // this.initData();
        },
		onLoad() {
			this.baseurl = this.$mStore.state.BaseUrl 
			this.doGetLocation()
			this.initData()
		},
        onShareAppMessage() {
            return {
                title: '欢迎来到XXX家政',
                path: '/pages/index/index'
            }
        },
        //下拉刷新
        onPullDownRefresh() {
            this.getIndexList('refresh');
        },
        methods: {
			target(url){
				uni.switchTab({
					url
				})		
			},
			//获取当前的位置
			doGetLocation(){
				uni.getLocation({
				    type: 'gcj02',
					geocode: true,
				    success: ((res) => {
				    	this.city = res.address.city;
				    })
				});
			},
			navTo(route) {
				console.log(route)
			      this.$mRouter.push({ route });
			},
			// 定位搜索
			positionSearch(){
				this.cityVisiable = !this.cityVisiable
			},
			async initData() { 
				// 获取新人推荐 
				await this.$http.get(`${categoryList}`, {newbee_recommend:1}).then(r => { 
					console.log(r.msg)
						if (r.status == 0){
							
                             if (r.msg.length > 0){
								 this.newbee_infolst = []
								 let thumbnail_portait = null;
								 for (let i =0; i < r.msg.length; i++){
									 thumbnail_portait = r.msg[i].thumbnail_portait
									  
									 console.log(thumbnail_portait)
									 this.newbee_infolst.push({
										name:r.msg[i].productname,
										thumbnail_portait:thumbnail_portait,
										age:r.msg[i].age?r.msg[i].age:'-',
										sex: r.msg[i].sex,
										position:r.msg[i].nation,
										time:"2",
										languages:r.msg[i].languages.join("  "),
										type:r.msg[i].rules[0].name,
										id:r.msg[i].id,
										category:r.msg[i].category,
										workyears:r.msg[i].workyears?r.msg[i].workyears:'-', 
									}) 
								 }
							 }
						}
						 
					}).catch((r) => {
						console.log(r) 
					})
				// 获取推荐阿姨
				await this.$http.get(`${categoryList}`, {recommend:1}).then(r => { 
						if (r.status == 0){
							console.log(r.msg)
                             if (r.msg.length > 0){
								 this.infolst = []
								 let thumbnail_portait = null;
								 for (let i =0; i < r.msg.length; i++){
									 thumbnail_portait = r.msg[i].thumbnail_portait
									  
									 this.infolst.push({
										name:r.msg[i].productname,
										thumbnail_portait:thumbnail_portait,
										age:r.msg[i].age?r.msg[i].age:'-',
										sex: r.msg[i].sex,
										position:r.msg[i].nation,
										time:"2",
										languages:r.msg[i].languages.join("  "),
										type:r.msg[i].rules[0].name,
										id:r.msg[i].id,
										category:r.msg[i].category,
										workyears:r.msg[i].workyears?r.msg[i].workyears:'-', 
									}) 
								 }
							 }
						}
						 
					}).catch((r) => {
						console.log(r) 
					})
				await this.$http.get(`${speadList}`, {}).then(r => {
						 
						if (r.status == 0){
                             if (r.msg.length > 0){
								 this.swiperImg = []
								 for (let i =0; i < r.msg.length; i++){
									 this.swiperImg.push(this.baseurl + r.msg[i].image)  
								 }
							 }
						}
						 
					}).catch((r) => {
						console.log(r) 
					})
				
			},
            // 监听轮播图切换
            handleDotChange(e) {
				this.swiperCurrent = e.detail.current;
            },
          
            
        
        }
	}
</script>

<style lang="scss" scoped>
	.rf-index {
		background-color: #FFFFFF;
		// width: 100%;		
		.header,.location{
			display: flex;
			align-items: center;
		}
		.header{
			// height: 169rpx;
			position: fixed;
			width: 100%;
			z-index: 100;
			background-color: #ffffff;
			padding-top: 70rpx;//设置距离顶部的距离
			justify-content: space-between;
		}
		.fonttext,.location_text,.search_more,.assure_text
		,.position_title,.titlename,.value_name,.box_name,
		.city_text{
			font-style: normal;
			font-weight: 400;
			letter-spacing: 0em;
		}
		.notice{
			.notice_img{
				margin-right: 38rpx;
				width: 22rpx;
				height: 21.54rpx;
			}
		}
		.main_content{
			position: relative;
			top: 100rpx;
			background-color: #ffffff;
		}
		/*轮播图*/		
		.swiper {
			padding-top: 20rpx;
			margin: 0 40rpx 0 40rpx;
			height: 468rpx;
			swiper {					
				width: 100%;
				height: 400rpx;
				.swiper-item {
					border-radius: 28rpx;
					image { 
						width: 100%;
						height: 400rpx;
						border-radius: 28rpx;
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
		.middle{
			height: 357.68rpx;
			margin-top: 20rpx;
			.ads{
				display: flex;				
				height: 180rpx;
				justify-content: space-between;
				.ads_left_img{
					margin-left: 40rpx;
					margin-right: 19rpx;
					height: 180rpx;
					width: 163px;
					border-radius: 16rpx;
					filter: drop-shadow(0px 2px 2px rgba(0,0,0,0.25));
				}
				.ads_img{
					height: 180rpx;
					width: 163px;
					border-radius: 16rpx;
					margin-right: 40rpx;
					filter: drop-shadow(0px 2px 2px rgba(0,0,0,0.25));
				}
			}
			.vip{
				height: 140rpx;
				margin: 20rpx 40rpx;
				.vip_img{
					height: 140rpx;
					width: 100%;
					filter: drop-shadow(0px 2px 2px rgba(0,0,0,0.25));
					border-radius: 54rpx;
				}
				.vip_img_top{
					z-index: 100;
					position: relative;
					top: -190rpx;
					left: -6.6rpx;
					width: 202rpx;
					height: 200rpx;
				}
			}
			
		}
		.gift_ads{
			height: 158rpx;
			.gift{
				display: flex;
				height: 140rpx;
				justify-content: space-between;
				.gift_left_img{
					margin-left:40rpx;
					margin-right: 19rpx;
					height: 140rpx;
					border-radius: 16rpx;
					filter: drop-shadow(0px 2px 2px rgba(0,0,0,0.25));
				}
				.gift_img{
					margin-right: 40rpx;
					height: 140rpx;
					border-radius: 16rpx;
					filter: drop-shadow(0px 2px 2px rgba(0,0,0,0.25));
				}
			}
		}	
		.aunt_bottom{
			.aunt_top{
				margin: 80rpx 0 10rpx 20rpx;
			}	
		}
		.bottom{
			display: flex;
			width: 100%;
			margin-left: 20rpx;
			.main_card{
				margin-right: 22rpx;
			}
		}
		.bottom_text{
			color: rgba(255, 141, 14, 1);
			border-bottom: 4rpx solid #ff8d0e;
		}
		.search,.search_more{
			display: flex;
			justify-content: center;
			align-items: center;			
		}
		.search{
			width: 100%;
		}
		.search_more{
			width: 160rpx;			
			height: 50rpx;
			margin: 22rpx 0;
			border: 2rpx solid #FF8D0E;
			color: #FF8D0E;
			box-sizing: border-box;
			border-radius: 8rpx;
			font-family: Tensentype MingSongJ-W4;
			font-size: 20rpx;
			line-height: 24rpx;
		}
		.new_bottom{
			.new_top{
				margin: 0 0 10rpx 20rpx;
			}	
		}
		.assure_one,.bottom_assure{
			display: flex;
			align-items: center;
			justify-content: center;
		}
		.bottom_assure{
			padding-bottom: 20rpx;
			margin-top: 86rpx;
			.assure_one{
				padding-right: 28rpx;
				.assure_text{
					padding-right: 8rpx;
					font-family: Tensentype MingSongJ-W4;
					font-size: 24rpx;
					line-height: 28rpx;
					color: #5b5b5b;
				}
				.bottom_img{
					width: 28rpx;
					height: 28rpx;
				}
			}
		}
		.demo-popup{
			.s-popup-wrap{
				height: 316rpx;
			}
		}
	}
</style>
