<template>
	<view class="rf-index">
		 <!-- 顶部信息 -->
		 <view class="header">
		 	<view class="header-top">
		 		<citySearch v-model="city" @doGetLocation="doGetLocation"/>
		 		<view class="notice" @tap="navTo('/pages/user/notice/notice')">
		 			<image class="notice_img" src="../../static/home/Notification.svg"></image>
		 		</view>
		 	</view>
		 	<view class="header-bottom">
		 		<view class="logo">
		 			<image class="logo_img" src="../../static/home/logo.png"></image>
		 		</view>
		 	</view>
		 </view>	
		 <scroll-view
		 	class="main_content"
		 	scroll-y
		 >
			<view class="swipe">
				<view class="swipe_text">好帮手到家</view>
				<view class="swipe_text">得品质生活</view>
			</view>
			<!-- 轮播图 -->
			<!--
			<view class="swiper">
				<swiper @change="handleDotChange" :Lautoplay="true" :current="swiperCurrent" :circular='true'>
					<swiper-item v-for="(item,index) in swiperImg" :key="index">
						<view class="swiper-item">
							<image :src="item" mode="aspectFill"/>
						</view>
					</swiper-item>					
				</swiper>
				-->
				<!-- 轮播指示点样式修改 -->
				<!--
				<view class="dots">
					<block v-for="(item,index) in swiperImg.length" :key="item">
					  <view class="dot" :class="index==swiperCurrent ? ' active' : ''"></view>
					</block>
				</view>
				-->
			<!--
			</view>
			-->

			<!-- 分类列表 -->
			<view class="fct-center">
			  <view
			    v-for="(item, index) of commenUseList"
			    :key="index"
			    class="common-function__item"
			    @tap="toCategory(item.id)"
			  >
			    <image
			      class="common-function__img"
			      :src="`../../static/home/${item.src}.png`"
			    />
			    <view class="fct-content">{{ item.name }}</view>
			  </view>
			</view>
			<!--
			<categoryLists @target="target" />
			-->
			<!--广告-->
			<view class="ads">
				<view class="ads_left">
					
				</view>
				<view class="ads_right">
					
				</view>
			</view>
			<!--分享礼品-->
			<view class="gift_ads">
				<view class="gift">
					<view class="gift_left">
						<view class="gift_text">保洁烹饪</view>
						<view class="gift_text">洁净温馨</view>
						<view class="gift_search">点击查看</view>
					</view>
					<view class="gift_right">
						<view class="gift_text">保洁烹饪</view>
						<view class="gift_text">洁净温馨</view>
						<view class="gift_search">点击查看</view>
					</view>
				</view>			
			</view>
			<!-- 阿姨推荐 -->
			<!--
			<view class="aunt_bottom" >
				<view class="aunt_top"><text class="bottom_text">阿姨推荐</text></view>
				<view class="bottom" >
					<mainCard class="main_card" :info="item" v-for="(item,index) in infolst" :key="index"/>
				</view>				
			</view>	
			<view class="search" @tap="target('/pages/category/category')">
				<view class="search_more">查看更多</view>
			</view>
			-->
			
			<!-- 新人推荐 -->
			<!--
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
			-->
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
				infolst:[],
				newbee_infolst:[],
				commenUseList: [
				  {
				    name: "母婴育儿",
				    id:38,
				    src: "category1",
				  },
				  {
				    name: "月嫂服务",
				    id: 27,
				    src: "category2",
				  },
				  {
				    name: "老年呵护",
				    id:40 ,
				    src: "category3",
				  },
				  {
				    name: "家政服务",
				    id:41,
				    src: "category4",
				  },
				  {
				    name: "钟点工服务",
				    id: 42,
				    src: "category5",
				  },
				  {
				    name: "搬家清洁",
				    id: 45,
				    src: "category6",
				  },
				]

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
			toCategory(id=0){
				if (id > 0){
				     uni.setStorageSync('categoryid',id);
				}
				this.target('/pages/category/category')
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
										nation:r.msg[i].nation,
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
										nation:r.msg[i].nation,
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
			
		}
		.header{
			// height: 169rpx;
			position: fixed;
			width: 100%;
			height: 204rpx;
			z-index: 100;
			flex-direction: column;
			padding-top:20rpx;
			background-color: #ffffff;
			background-image: url(../../static/home/header.png);
			background-repeat: no-repeat;
			background-size: 100% 100%;
			justify-content: space-between;
			padding-bottom: 65rpx;
		}
		.header-top{
			display: flex;
			width: 100%;
			justify-content: space-between;
		}
		.header-bottom{
			display: flex;
			flex-direction: column;
			align-items: center;
		}
		.fonttext,.location_text,.search_more,.assure_text
		,.position_title,.titlename,.value_name,.box_name,
		.city_text{
			font-style: normal;
			font-weight: 400;
			letter-spacing: 0em;
		}
		.notice{
			display: flex;
			.notice_img{
				color: #FFFFFF;
				margin-right: 38rpx;
				width: 27rpx;
				height: 26.54rpx;
			}
		}
		.logo{
			display: flex;
			align-items: center;
			.logo_img{
				width: 288rpx;
				height: 74rpx;
			}
		}
		.main_content{
			position: relative;
			top: 100rpx;
			background-color: #ffffff;
		}
		.swipe{
			display: flex;
			flex-direction: column;
			padding-top: 200rpx;
			padding-left: 50rpx;
			width: 100%;
			height: 606rpx;
			color: #FF9700;
			font-weight: bold;
			color: red;
			border: 1rpx solid red;
			background-image: url(../../static/home/swipe.png);
			background-repeat: no-repeat;
			background-size: 100% 100%;
			.swipe_text{
				color: #FF9700;
				font-weight: bold;
				font-size: 40rpx;
			}
		}
		.fct-center {
		  display: flex;
		  justify-content: space-between;
		  flex-flow: wrap row;
		  padding: 20rpx 0rpx 50rpx;
		  margin:0rpx 35rpx;
		  background-color: #FFFFFF;
		  border-top: 15rpx solid #FF8000;
		  border-left:1rpx solid #909399;
		  border-right:1rpx solid #909399;
		  border-bottom:1rpx solid #909399;
		  border-bottom-left-radius: 26rpx;
		  border-bottom-right-radius: 26rpx;
		  margin-top: -50rpx;
		  margin-bottom: 40rpx;
		  .common-function__item {
		    flex: 0 0 15%;
		    display: flex;
		    justify-content: center;
		    align-items: center;
		    flex-flow: wrap column;
		    .common-function__img {
		      width: 60rpx;
		      height: 60rpx;
		    }
		    .fct-content {
			  font-weight: bold;
		      font-size: 20rpx;
		      line-height: 28rpx;
		      display: flex;
		      align-items: flex-end;
		      color: #FF9700;
		      margin-top: 10rpx;
			  margin-left: 1rpx;
			  padding-top: 10rpx;
		    }
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
		.ads{
			display: flex;				
			height: 242rpx;
			margin-bottom: 30rpx;
			flex-direction: row;
			justify-content: space-between;
			.ads_left{
				display: flex;
				margin-left: 40rpx;
				height: 224rpx;
				width: 332px;
				border-radius: 16rpx;
				background-image: url(../../static/home/middle_left.png);
				background-repeat: no-repeat;
				background-size: 100% 100%;
			}
			.ads_right{
				display: flex;
				margin-right: 40rpx;
				height: 224rpx;
				width: 448px;
				border-top: 15rpx solid #FF8000;
				margin-left: -20rpx;
				border-bottom-left-radius: 50rpx;
				background-image: url(../../static/home/middle_right.png);
				background-repeat: no-repeat;
				background-size: 100% 90%;
				background-position: left bottom;
			}
		}
		.gift_ads{
			height: 300rpx;
			width: 100%;
			.gift{
				display: flex;
				height: 280rpx;
				width: 100%;
				justify-content: space-between;
				.gift_left{
					display: flex;
					flex-direction: column;
					margin-left: 40rpx;
					margin-right:20rpx;
					width: 48%;
					height: 100%;
					border-top: 15rpx solid #FF8000;
					background-image: url(../../static/home/gift_left.png);
					background-repeat: no-repeat;
					background-size: 203rpx 245rpx ;
					background-position: right bottom;
					padding-top: 15rpx;
					.gift_text{
						font-size: 24rpx;
						padding-left: 15rpx;
						color: #FF9700;
						font-weight: bold;
					}
					.gift_search{
						width: 80rpx;
						height: 30rpx;
						text-align: center;
						margin-left: 15rpx;
						background-color:#FF8000;
						padding-top: 5rpx;
						border-radius: 20rpx;
						font-size: 15rpx;
						color: #FFFFFF;
					}
				}
				.gift_right{
					display: flex;
					flex-direction: column;
					margin-right: 40rpx;
					margin-left:20rpx;
					width: 48%;
					height: 100%;
					border-top: 15rpx solid #FF8800;
					background-image: url(../../static/home/gift_right.png);
					background-repeat: no-repeat;
					background-size: 252rpx 245rpx ;
					background-position: right bottom;
					padding-top: 15rpx;
					.gift_text{
						font-size: 24rpx;
						padding-left: 15rpx;
						color: #FF9700;
						font-weight: bold;
					}
					.gift_search{
						width: 80rpx;
						height: 30rpx;
						text-align: center;
						margin-left: 15rpx;
						background-color:  #FF8800;
						padding-top: 5rpx;
						border-radius: 20rpx;
						font-size: 15rpx;
						color: #FFFFFF;
					}
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
