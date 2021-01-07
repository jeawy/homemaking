<template>
	<view class="content">
		<!--导航栏-->
		<view class="top">
			<view class="navbar-top">
				<view class="navbar-top-text">我的订单</view>
				<view class="navbar-top-img">
					<image class="search-img" src="/static/order/search.svg"></image>
					<image class="erji-img" src="/static/order/erji.svg"></image>
					<image class="message-img" src="/static/order/orange_messages.svg"></image>
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
				<orderCard :orderList="allOrderList"/>
			</swiper-item>
			<!--待付款订单页面-->
			<swiper-item>
				<orderCard :orderList="unPayList"/>
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
				<orderCard :orderList="finishedList"/>
			</swiper-item>
			<!--待评价订单页面-->
			<swiper-item>
				<orderCard :orderList="unEvalList"/>
			</swiper-item>
		</swiper>
		<!-- <rf-loading v-if="loading"></rf-loading> -->
	</view>
</template>

<script>
	import mainCard from '@/components/main-card.vue';
	import orderCard from '@/components/order-card.vue';
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
					},
					{
						image:"../../static/order/zhang.svg",
						name:"艾米",
						content:"小时工 - 清洁",
						ispay:0,
						money:60,
						num:1
					},
					{
						image:"../../static/order/zhang.svg",
						name:"张阿姨",
						content:"包全天家政",
						ispay:1,
						money:400,
						num:2
					},
					{
						image:"../../static/order/zhang.svg",
						name:"张阿姨",
						content:"包全天家政",
						ispay:0,
						money:400,
						num:4
					},
					{
						image:"../../static/order/zhang.svg",
						name:"艾米",
						content:"小时工 - 清洁",
						ispay:1,
						money:60,
						num:1
					},{
						image:"../../static/order/zhang.svg",
						name:"艾米",
						content:"小时工 - 清洁",
						ispay:1,
						money:60,
						num:1
					},{
						image:"../../static/order/zhang.svg",
						name:"艾米",
						content:"小时工 - 清洁",
						ispay:1,
						money:60,
						num:1
					}
				],
				// 未付款订单
				unPayList:[
					{
						image:"../../static/order/zhang.svg",
						name:"艾米",
						content:"小时工 - 清洁",
						ispay:0,
						money:60,
						num:1
					},
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
				// 已完成订单
				finishedList:[
					{
						image:"../../static/order/zhang.svg",
						name:"张阿姨",
						content:"包全天家政",
						ispay:1,
						money:400,
						num:3
					}
				],
				// 待评价订单
				unEvalList:[
					{
						image:"../../static/order/zhang.svg",
						name:"张阿姨",
						content:"包全天家政",
						ispay:1,
						money:400,
						num:3
					}
				],
				loading: true,
			};
		},
		onShow(){
		},
		onLoad(options){

		},
		methods: {
			// 监听swiper切换
			changeTab(e){
				console.log(e.target.current)
				this.tabCurrentIndex = e.target.current;
			},
			// 顶部tab点击
			tabClick(index){
				this.tabCurrentIndex = index;
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
/*订单列表*/
.order-list {
	// margin-top:4rpx;
	// margin-top:216rpx;
	margin-left:30rpx;
	.order-item{
		margin-bottom:20rpx;
		height: 240rpx;
		width:690rpx;
		background-color:#FFFFFF;
		border-radius: 8rpx;
		font-family: Tensentype MingSongJ-W4;
		font-style: normal;
		font-weight: normal;
		.order-item-img{
			position: absolute;
			border-radius: 8rpx;
			width:120rpx;
			height: 120rpx;
			margin-top:24rpx;
			margin-left:10rpx
		}
		.order-item-name{
			position: absolute;
			margin-left: 150rpx;
			margin-top: 24rpx;
			width: 72rpx;
			height: 28rpx;
			font-size: 24rpx;
			color: #515151;
		}
		.order-item-content{
			position: absolute;
			height: 24rpx;
			margin-left: 150rpx;
			margin-top: 72rpx;
			font-size: 20rpx;
			color: #888888;

		}
		.order-item-ispay{
			position: absolute;
			height: 22rpx;
			right: 38rpx;
			margin-top: 30rpx;
			font-size: 18rpx;
			color: #FF8D0E;
		}
		.activeColor{
			color: #FF480E;
		}
		.order-item-money{
			position: absolute;
			height: 24rpx;
			right: 44rpx;
			margin-top: 72rpx;
			font-size: 20rpx;
			color: #515151;
		}
		.order-item-num{
			position: absolute;
			width: 26rpx;
			height: 24rpx;
			right: 38rpx;
			margin-top: 96rpx;
			font-size: 20rpx;
			color: #888888;
		}
		.order-item-price{
			position: absolute;
			height: 28rpx;
			right: 40rpx;
			margin-top: 140rpx;
			font-size: 24rpx;		
			color: #515151;
		}
		.order-item-side{
			position: absolute;
			color: #FF8D0E;
			display: flex;
			flex-wrap: wrap;
			margin-top:188rpx;
			// padding-left: 258rpx;
			right: 40rpx;
			.btn{
				margin-left:20rpx;
				font-size: 20rpx;
				padding: 8rpx 22rpx;
				border: 2rpx solid #FF8D0E;
				box-sizing: border-box;
				border-radius: 4rpx;
			}
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

