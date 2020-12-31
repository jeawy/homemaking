<template>
	<view class="content">
		<!--导航栏-->
		<view class="navbar">
			<view
				v-for="(item, index) in navList" :key="index"
				class="navitem"
				:class="{current: tabCurrentIndex === index}"
				@tap="tabClick(index)"
			>
				{{item.value}}
			</view>
		</view>
		<swiper :current="tabCurrentIndex" class="swiper-box" duration="300" @change="changeTab">
			<!--全部订单页面-->
			<swiper-item>
				<view class="swiper-item">
					<scroll-view
						class="list-scroll-content"
						scroll-y
					>
						<view class="order-list">
							<view
								class="order-item"
								v-for="(item, index) in allOrderList"
								:key="index"
							>
								<image class="order-item-img" :src="item.image"></image>
								<view class="order-item-name">{{item.name}}</view>
								<view class="order-item-content">{{item.content}}</view>
								<view class="order-item-ispay" 
									:class="{activeColor: item.ispay==0}">
									{{item.ispay==0?"未付款":"已付款"}}
								</view>
								<view class="order-item-money">${{item.money}}</view>
								<view class="order-item-num">x{{item.num}}</view>
								<view class="order-item-price">
									总价
								  <span style="color:#FF8D0E;font-size:20rpx">${{item.money * item.num}}</span>
								</view>
								<view class="order-item-side">
									<view class="btn">删除订单</view>
									<view class="btn">申请单据</view>
									<view class="btn">发表评论</view>
								</view>
							</view>
						</view>
					</scroll-view>
				</view>
			</swiper-item>
			<!--待付款订单页面-->
			<swiper-item>
				<view class="swiper-item">
					<scroll-view
						class="list-scroll-content"
						scroll-y
					>
						<view class="order-list">
							<view
								class="order-item"
								v-for="(item, index) in unPayList"
								:key="index"
							>
								<image class="order-item-img" :src="item.image"></image>
								<view class="order-item-name">{{item.name}}</view>
								<view class="order-item-content">{{item.content}}</view>
								<view class="order-item-ispay" 
									:class="{activeColor: item.ispay==0}">
									{{item.ispay==0?"未付款":"已付款"}}
								</view>
								<view class="order-item-money">${{item.money}}</view>
								<view class="order-item-num">x{{item.num}}</view>
								<view class="order-item-price">
									总价
								  <span style="color:#FF8D0E;font-size:20rpx">${{item.money * item.num}}</span>
								</view>
								<view class="order-item-side">
									<view class="btn">删除订单</view>
									<view class="btn">申请单据</view>
									<view class="btn">发表评论</view>
								</view>
							</view>
						</view>
					</scroll-view>
				</view>
			</swiper-item>
			<!--进行中订单页面-->
			<swiper-item class="process-order">
				<image class="process-img" src="../../static/order/order.svg"></image>
				<view class="process-text">您还没有相关的订单哦</view>
				<view class="process-tip">猜您想找的人和服务</view>
				<view class="server-info">
					<image class="portrait-info" src="../../static/order/Ellipse.svg"></image>
				</view>
			</swiper-item>
			<!--已完成订单页面-->
			<swiper-item>
				<view class="swiper-item">
					<scroll-view
						class="list-scroll-content"
						scroll-y
					>
						<view class="order-list">
							<view
								class="order-item"
								v-for="(item, index) in finishedList"
								:key="index"
							>
								<image class="order-item-img" :src="item.image"></image>
								<view class="order-item-name">{{item.name}}</view>
								<view class="order-item-content">{{item.content}}</view>
								<view class="order-item-ispay" 
									:class="{activeColor: item.ispay==0}">
									{{item.ispay==0?"未付款":"已付款"}}
								</view>
								<view class="order-item-money">${{item.money}}</view>
								<view class="order-item-num">x{{item.num}}</view>
								<view class="order-item-price">
									总价
								  <span style="color:#FF8D0E;font-size:20rpx">${{item.money * item.num}}</span>
								</view>
								<view class="order-item-side">
									<view class="btn">删除订单</view>
									<view class="btn">申请单据</view>
									<view class="btn">发表评论</view>
								</view>
							</view>
						</view>
					</scroll-view>
				</view>
			</swiper-item>
			<!--待评价订单页面-->
			<swiper-item>
				<view class="swiper-item">
					<scroll-view
						class="list-scroll-content"
						scroll-y
					>
						<view class="order-list">
							<view
								class="order-item"
								v-for="(item, index) in unEvalList"
								:key="index"
							>
								<image class="order-item-img" :src="item.image"></image>
								<view class="order-item-name">{{item.name}}</view>
								<view class="order-item-content">{{item.content}}</view>
								<view class="order-item-ispay" 
									:class="{activeColor: item.ispay==0}">
									{{item.ispay==0?"未付款":"已付款"}}
								</view>
								<view class="order-item-money">${{item.money}}</view>
								<view class="order-item-num">x{{item.num}}</view>
								<view class="order-item-price">
									总价
								  <span style="color:#FF8D0E;font-size:20rpx">${{item.money * item.num}}</span>
								</view>
								<view class="order-item-side">
									<view class="btn">删除订单</view>
									<view class="btn">申请单据</view>
									<view class="btn">发表评论</view>
								</view>
							</view>
						</view>
					</scroll-view>
				</view>
			</swiper-item>
		</swiper>
		<!-- <rf-loading v-if="loading"></rf-loading> -->
	</view>
</template>

<script>
	import moment from '@/common/moment';
	import mConstData from '@/config/constData.config'
	export default {
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
					},
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
				progressList:[
					{
						image:"../../static/order/zhang.svg",
						name:"张阿姨",
						content:"包全天家政",
						ispay:1,
						money:400,
						num:3
					}
				],
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
	height: calc(100% - 40px);
}
.uni-swiper-item{
	height: auto;
}
/*顶部导航*/
.navbar{
	font-size:24rpx;
	padding-top:24rpx;
	.navitem{
		margin-left:66rpx;
	}
	.current{
		color: #ff8d0e;
	}
}
/*订单列表*/
.order-list {
	margin-top:4rpx;
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
			width: 54rpx;
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
		position: absolute;
		margin-left: 34.67%;
		top: 7.61%;
	}
	.process-text{
		position: absolute;
		margin-left: 36.8%;
		top: 32.33%;
		font-size: 20rpx;
		line-height: 24rpx;
		color: #888888;

	}
	.process-tip{
		position: absolute;
		margin-left: 36.8%;
		top: 42.16%;
		font-size: 20rpx;
		line-height: 24rpx;
		color: #363636;
	}
	.server-info{
		.portrait-info{
			position: absolute;
			width:306rpx;
			height:306rpx;
			left: 5.07%;
			top: 47.54%;
			background:rgba(239, 243, 246, 0.75);
		}
	}
}
</style>

