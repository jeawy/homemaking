<template>
	<view class="content">
		<!--导航栏-->
		<view class="top">
			<view class="navbar-top">
				<view class="navbar-top-text">我的订单</view>
				<view class="navbar-top-img">
					<image class="search-img" @tap="navTo('/pages/index/search/search')" src="/static/order/search.svg"></image>
					<image class="erji-img" src="/static/order/erji.svg"></image>
					<image @tap="navTo('/pages/user/notice/notice')"
					 class="message-img" src="/static/order/orange_messages.svg"></image>
				</view>
			</view>
			<view class="navbar-bottom">
				<view
					v-for="(item, index) in navList" :key="index"
					class="navitem"
					:class="{current: tabCurrentIndex === index}"
					@tap="tabClick(index)"
				>
					{{item.value}}
				</view>
			</view>
		</view>
		<swiper :current="tabCurrentIndex" class="swiper-box" duration="300" @change="changeTab">
			<!--全部订单页面-->
			<swiper-item>
				<!--
				<orderCard :orderList="orderList" :productList="productList" 
				:orderStatus="orderStatus" @getOrderList="getOrderList"/>-->
				<orderCard :orderList="orderList" @getOrderList="getOrderList"/>
			</swiper-item>
			<!--待付款订单页面-->
			<swiper-item>
				<!--
				<orderCard :orderList="orderList" :productList="productList" 
				:orderStatus="orderStatus" @getOrderList="getOrderList"/>-->
				<orderCard :orderList="orderList" @getOrderList="getOrderList"/>
			</swiper-item>
			<!--进行中订单页面-->
			<swiper-item class="process-order">
				<image class="process-img" src="../../static/order/order.svg"></image>
				<view class="process-text">您还没有相关的订单哦</view>
				<view class="process-tip">猜您想找的人和服务</view>
				<view class="process-bottom" >
					<mainCard class="main_card" :info="item" v-for="(item,index) in processList" :key="index"/>
				</view>
			</swiper-item>
			<!--已完成订单页面-->
			<swiper-item>
				<!--
				<orderCard :orderList="orderList" :productList="productList" 
				:orderStatus="orderStatus" @getOrderList="getOrderList"/>-->
				<orderCard :orderList="orderList" @getOrderList="getOrderList"/>
			</swiper-item>
			<!--待评价订单页面-->
			<swiper-item>
				<!--
				<orderCard :orderList="orderList" :productList="productList" 
				:orderStatus="orderStatus" @getOrderList="getOrderList"/>-->
				<orderCard :orderList="orderList" @getOrderList="getOrderList"/>
			</swiper-item>
		</swiper>
		<rf-loading v-if="loading"></rf-loading>
	</view>
</template>

<script>
	import mainCard from '@/components/main-card.vue';
	import orderCard from '@/components/order-card.vue';
	import {orderList} from '@/api/userInfo';
	export default {
		components: {
			mainCard,
			orderCard
        },
		data() {
			return {
				tabCurrentIndex: 0,
				loadingType: 'more',
				navList: [
					{
						value:"全部",
					},{
						value:"待付款",
					},{
						value:"进行中",
					},{
						value:"已完成",
					},{
						value:"待评价",
					}
				],
				// 所有订单
				allOrderList: [
					{
						image:"../../static/order/zhang.svg",
						name:"张阿姨",
						content:"包全天家政",
						ispay:1,
						money:400,
						num:3
					}
				],
				// 进行中订单
				processList:[{
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
				loading: true,
				hasLogin:true,
				orderList:[],
				productList:[],
				page:1,
				orderStatus:[]
			};
		},
		onShow(){
			// #ifdef H5 || APP-PLUS
			this.page = 1;
			this.orderList.length = 0;
			this.initData();
			// #endif
		},
		onLoad(options){

		},
		methods: {
			// 监听swiper切换
			changeTab(e){
				this.tabCurrentIndex = e.target.current;
				this.loading = true;
				this.getOrderList();
				console.log('tab变换');
			},
			// 顶部tab点击
			tabClick(index){
				this.tabCurrentIndex = index;
				this.loading = true;
				this.getOrderList();
			},
			navTo(route){
				if (!route) {
                    return;
				}
				if (!this.hasLogin) {
                    uni.showModal({
                        content: '你暂未登陆，是否跳转登录页面？',
                        success: (confirmRes) => {
                            if (confirmRes.confirm) {
                                this.$mRouter.push({route: '/pages/public/logintype'});
                            }
                        }
                    });
                } else {
					this.$mRouter.push({route});
                }
			},
			// 数据初始化
			initData () {
				this.getOrderList();
			},
			// 订单状态
			statusformat(status) {
				switch (status) {
					case "0":
						return "待付款";
					case "1":
						return "待发货";
					case "2":
						return "已发货";
					case "3":
						return "已收货";
					case "4":
						return "已完成";
					case "-1":
						return "退货申请";
					case "-2":
						return "退款中";
					case "-3":
						return "已退货";
					case "-4":
						return "已关闭";
					case "-5":
						return "撤销申请";
				}
			},
			// 获取订单列表
			async getOrderList(type) {
				this.loading = true;
				let params = {};
				let index = this.tabCurrentIndex;
				if (this.tabCurrentIndex) {
					if(this.tabCurrentIndex === 3 || this.tabCurrentIndex === 4){
						params.status = 3;
					}else{
						params.status = index - 1;
					}
				}
				params.page = this.page;
				await this.$http.get(`${orderList}`, params).then(async r => {
					if (type === 'refresh') {
						uni.stopPullDownRefresh();
					}
					//this.loadingType  = r.data.length === 10 ? 'more' : 'nomore';
					this.orderList = [ ...r.msg];
					/*
					this.orderList.map(item=>{
						this.productList.push(item.rules.thumbnail_portait);
						this.orderStatus.push(this.statusformat(item.status));
					})*/
					// if (this.orderList.length === 0) {
				    // await this.getGuessYouLikeList();
					// }
					this.loading = false;
				}).catch(() => {
					this.loading = false;
					if (type === 'refresh') {
						uni.stopPullDownRefresh();
					}
				});
			},
		}
	}
</script>

<style lang="scss">
page, .content{
	background: $page-color-base;
	height: 100%;
}
.swiper-box{
	height: 100%;
}
.uni-swiper-item{
	height: auto;
}
.list-scroll-content{
	height: 100vh;
	padding-top: 178rpx;//设置距离顶部的距离
}
/*顶部导航*/
.top{
	// height: 212rpx;
	width:100%;
	background: #FFFFFF;
    position: fixed;
	z-index: 10;
	padding-top: 70rpx;//设置距离顶部的距离
	padding-bottom: 20rpx;
	.navbar-top{
		display: flex;
		// margin-top: 108rpx;
		height: 28rpx;
		margin-bottom:24rpx;
		.navbar-top-text{
			margin-left: 164px;
			font-size: 24rpx;
			color: #515151;
		}
		.navbar-top-img{
			margin-left:182rpx;
			.search-img{
				width: 21.8rpx;
				height: 22rpx;
			}
			.erji-img{
				margin-left:20.2rpx;
				width: 21.1rpx;
				height: 22rpx;
			}
			.message-img{
				margin-left:20.9rpx;
				width: 20.52rpx;
				height: 20.08rpx;
			}
		}
	}
	.navbar-bottom{
		display: flex;
		background: #fff;
		height: 32rpx;
		font-size:24rpx;
		// margin-top:24rpx;
		// margin-bottom:28rpx;
		line-height: 46rpx;
		.navitem{
			margin-left:66rpx;
			margin-right: 24rpx;
			font-size:20rpx;
			color:#888888;
		}
		.current{
			color: #ff8d0e;
			border-bottom:2rpx solid #FF8D0E;
		}
	}
}
/*进行中列表*/
.process-order{
	.process-img{
		width:232rpx;
		height:284rpx;
		margin-left: 34.67%;
		margin-top: 286rpx;
	}
	.process-text{
		margin-left: 36.8%;
		margin-top: 28rpx;
		font-size: 20rpx;
		line-height: 24rpx;
		color: #888888;

	}
	.process-tip{
		margin-left: 36.8%;
		margin-top: 100rpx;
		margin-bottom: 20rpx;
		font-size: 20rpx;
		line-height: 24rpx;
		color: #363636;
	}
	.process-bottom{
		display: flex;
		width: 100%;
		margin-left: 20rpx;
		.main_card{
			margin-right: 22rpx;
		}
	}
}
</style>

