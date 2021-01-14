<template>
	<view class="user">
		<!--顶部导航栏-->
		<view class="user-navbar">
			<view class="setting">
			   <image class="setting_img" src="/static/my/set.svg"></image>  
			</view>
			<view class="notice" @tap="navTo(userInfo ? '/pages/user/notice/notice' : 'login')">
				<image class="notice_img" src="/static/my/white_message.svg"></image>	  
			</view>
		</view>
		<!--头部-->
		<view class="user-section">
			<view class="middle" @tap="navTo(userInfo ? '/pages/user/userinfo/userinfo' : 'login')">
				<!--用户头像-->
				<view class="portrait_bg">
					<image class="portrait"
						:src="userInfo.thumbnail_portait || headImg"></image>
				</view>
				 
				<!--账户信息-->
				<view class="account-info">
					<view class="username">
						<text>
							{{ userInfo.username || userInfo.phone||'请先登录'}}
						</text>
						<image class="sign"
							:src="account_sign"></image>
					</view>
					<text class="account">
						账号：{{ userInfo.phone  }} 
					</text>
				</view>
				<!--右箭头-->
				<view class="arrow">
					<image class="arrow-img" src="../../static/my/arrow_right_white.svg"></image>
				</view>
			</view>
			<!--关注的人-->
			<view class="bottom">
				<view v-for="(item,index) in attentList" :key="index">
					<view class="tj-item">
						<view class="num">
							{{ item.num }}
						</view>
						<view class="content">{{item.text}}</view>
					</view>
				</view>
			</view>		 
		</view>
		<!-- 个人中心 内容区-->
		<view class="user-content">
			<!--我的订单-->
			<view class="my-order">
				<view class="order-top" @tap="target('/pages/cart/cart')">
					<view class="order-name">我的订单</view>
					<view class="look-order">查看全部订单</view>
					<image class="order-arrow-img" src="../../static/my/arrow_right_orange.svg"></image>
				</view>
				<view class="order-center">
					<view v-for="(item,index) in orderList" :key="index">
						<view class="order-center-item" @tap="target('/pages/cart/cart?tab='+item.index)">
							<image class="nopay-img" :src="item.image"></image>
							<view class="order-content">{{item.value}}</view>
						</view>
					</view>
					<!-- <view class="order-center-two">
						<image class="nopay-img" src="../../static/my/ongoing.svg"></image>
						<view class="order-content">进行中</view>
					</view>
					<view class="order-center-two">
						<image class="nopay-img" src="../../static/my/finished.svg"></image>
						<view class="order-content">已完成</view>
					</view>
					<view class="order-center-two">
						<image class="nopay-img" src="../../static/my/unreviews.svg"></image>
						<view class="order-content">待评论</view>
					</view> -->
				</view>
			</view>
			<!--常用功能-->
			<view class="common-function">
				<view class="fct-top">
					<view class="fct-name">常用功能</view>
				</view>
				<view class="fct-center">
					<view class="fct-center-one fct-center-all" @tap="navTo('/pages/user/address/address')">
						<image class="address-img" src="../../static/my/address.svg"></image>
						<view class="fct-content">常用地址</view>
					</view>
					<view class="fct-center-two fct-center-all">
						<image class="payment-img" src="../../static/my/payment.svg"></image>
						<view class="fct-content">付款方式</view>
					</view>
					<view class="fct-center-three fct-center-all">
						<image class="setting-img" src="../../static/my/settings.svg"></image>
						<view class="fct-content">基本设置</view>
					</view>
					<view class="fct-center-four fct-center-all">
						<image class="vip-img" src="../../static/my/vip.svg"></image>
						<view class="fct-content">会员专区</view>
					</view>
					<view class="fct-center-five fct-center-all">
						<image class="share-img" src="../../static/my/share.svg"></image>
						<view class="fct-content">分享有礼</view>
					</view>
					<view class="fct-center-six fct-center-all" 
						@tap="navTochat('/pages/user/chat/chat')">
						<image class="service-img" src="../../static/my/service.svg"></image>
						<view class="fct-content">在线客服</view>
					</view>
					<view class="fct-center-seven fct-center-all">
						<image class="interview-img" src="../../static/my/interview.svg"></image>
						<view class="fct-content">视频面试</view>
					</view>
					<view class="fct-center-eight fct-center-all">
						<image class="user-img" src="../../static/my/userdocument.svg"></image>
						<view class="fct-content">用户协议</view>
					</view>
					<view class="fct-center-eight fct-center-all" @tap="navTo('/pages/user/my-appointment/index')">
						<image class="user-img" src="../../static/my/userdocument.svg"></image>
						<view class="fct-content">我的预约</view>
					</view>
				</view>
			</view>
			<!-- 浏览历史 -->
			<view class="history-section">
				<!-- <list-cell icon="iconlishijilu" iconColor="#FFAB4E" @eventClick="navTo('/pages/user/footprint/footprint')"
				           title="我的足迹"></list-cell> -->
				<view class="fct-top" @tap="navTo('/pages/user/footprint/footprint')">
					<view class="fct-name">我的足迹</view>
					<image class="arrow-img" src="../../static/my/arrow_right_orange.svg"></image>
				</view>
				<view v-if="hasLogin">
					<scroll-view scroll-x class="h-list" v-if="footPrintList.length > 0">
						<view class="h-item" v-for="item in footPrintList" :key="item.id">
							<image class="h-item-img" @tap.stop="navTo(`/pages/product/product?id=${item.product.id}`)"
							       :src="item.product.picture" mode="aspectFill"></image>
						<view class="h-item-text in2line">{{ item.product.name }}</view>
						</view>
					</scroll-view>
					<view class="no-foot-print" v-else-if="footPrintList.length === 0" @tap="navTo('/pages/product/list')">
						<i class="iconfont iconshare no-foot-print-icon"/>
						先去浏览一些吧~
					</view>
				</view>
				<view class="no-foot-print" v-else @tap="navTo('/pages/user/footprint/footprint')">
					<i class="iconfont iconmima no-foot-print-icon"/>
					登陆后查看
				</view>
			</view>
		</view>
	</view>
</template>
<script>
import {footPrintList, memberInfo} from '@/api/userInfo';
import userinfoVue from './userinfo/userinfo.vue';
import listCell from '@/components/rf-list-cell';
    export default {
		components: {
		    listCell
		},
        data() {
            return {
				message:"../../static/my/white_message.svg",
				setting:"../../static/my/set.svg",
				user_portrait_bg:"../../static/my/portrait_bg.svg",
				user_portrait:"../../static/my/portrait.svg",
				username:"---",
				account:"---",
				account_sign:"../../static/my/king.svg",
				focusernum:2,
				focuservicenum:8,
				settingList: this.$mConstDataConfig.settingList,
                orderSectionList: this.$mConstDataConfig.orderSectionList,
                amountList: this.$mConstDataConfig.amountList,
                promotionList: this.$mConstDataConfig.promotionList,
				footnum:35,
				couponum:4,
				headImg: this.$mAssetsPath.headImg,
				attentList:[
					{
						num:2,
						text:"关注的人"
					},
					{
						num:8,
						text:"关注服务"
					},
					{
						num:35,
						text:"足迹"
					},
					{
						num:4,
						text:"红包卡券"
					}
				],
				orderList:[
					{
						image:"../../static/my/nopay.svg",
						value:"待付款"
					},
					{
						image:"../../static/my/ongoing.svg",
						value:"进行中"
					},
					{
						image:"../../static/my/finished.svg",
						value:"已完成"
					},
					{
						image:"../../static/my/unreviews.svg",
						value:"待评论"
					}
				],
				userInfo: { // 用户信息
                    promoter: null  // 分销商信息
				},
				hasLogin:true,
				footPrintList:[]
            }
		},
		onLoad(){
			// 
			// this.userInfo = this.$mStore.state.userInfo
		},
		async onShow() {
            // 初始化数据
            this.initData();
        },
        methods: {
			async initData() {
				this.hasLogin = this.$mStore.getters.hasLogin;
				console.log(this.hasLogin)
                if (this.hasLogin) {
                    // await this.getMemberInfo();
					//await this.initCartItemCount();
					console.log(this.$mStore.state.userInfo)
					this.userInfo = this.$mStore.state.userInfo
                } else {
                    this.loading = false;
                    this.resetSectionData();
                }
            },
			// 给个人中心的各模块赋值
            setSectionData(data) {
                const orderSynthesizeNumArr = [];
                for (let item in data.order_synthesize_num) {
                    orderSynthesizeNumArr.push(data.order_synthesize_num[item])
                }
                for (let i = 0; i < this.orderSectionList.length; i++) {
                    this.orderSectionList[i].num = orderSynthesizeNumArr[i].toString();
                }
                this.amountList[0].value = data.account.user_money || 0;
                this.amountList[1].value = data.coupon_num || 0;
                this.amountList[2].value = data.account.user_integral || 0;
                this.promotionList[0].value = data.promoter && data.promoter.accumulate_brokerage || 0;
                this.promotionList[1].value = data.promoter && data.promoter.user_brokerage || 0;
                this.promotionList[2].value = data.promoter && data.promoter.amount_drawn_brokerage || 0;
                // 更新userInfo缓存
				//console.log(data)
                
            },
			 async getMemberInfo() {
                await this.$http.get(memberInfo).then(async r => {
                    this.loading = false;
					this.userInfo = r.data;
					uni.setStorageSync('userInfo', r.data);
                    //await uni.setStorageSync('cartNum', r.data.cart_num);
                    // 获取足迹列表
                    await this.getFootPrintList();
					await this.setSectionData(r.data);
                }).catch(() => {
                	  this.hasLogin = false;
                	  this.userInfo = {};
                    this.resetSectionData();
                    this.loading = false;
                });
			},
			// 设置购物车数量角标
            async initCartItemCount() {
							if (this.hasLogin && parseInt(uni.getStorageSync('cartNum'), 10) > 0) {
                await uni.setTabBarBadge({
                  index: this.$mConstDataConfig.cartIndex,
                  text: uni.getStorageSync('cartNum')
                });
							} else {
                uni.removeStorageSync('cartNum');
                uni.removeTabBarBadge({index: this.$mConstDataConfig.cartIndex});
							}
            },
			// 清空个人中心的各模块状态
            resetSectionData() {
                uni.removeTabBarBadge({index: this.$mConstDataConfig.cartIndex});
                this.amountList[0].value = 0;
                this.amountList[1].value = 0;
                this.amountList[2].value = 0;
                this.promotionList[0].value = 0;
                this.promotionList[1].value = 0;
                this.promotionList[2].value = 0;
                this.orderSectionList[0].num = 0;
                this.orderSectionList[1].num = 0;
                this.orderSectionList[2].num = 0;
                this.orderSectionList[3].num = 0;
                this.orderSectionList[4].num = 0;
            },
			// 获取足迹列表
            async getFootPrintList() {
                await this.$http.get(`${footPrintList}`).then(r => {
					//console.log(r.data)
                    this.footPrintList = r.data
                });
            },
			navTochat(url){
				uni.navigateTo({
					url
				})
			},
			// tabbar页面跳转（查看更多）
			target(url){
				uni.switchTab({
					url
				})				
			},
			// 统一跳转接口,拦截未登录路由
            navTo(route) {
				//console.log(route)
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
        }
    }
</script>
<style lang='scss' scoped>
.user {
	font-family: Tensentype MingSongJ-W2;
	font-style: normal;
	font-weight: normal;
	/*顶部导航栏*/
	.user-navbar{
		display: flex;
		position: absolute;
		left: 86.67%;
		top: 6.9%;
		.setting{
			.setting_img{
				width: 20.86rpx;
				height: 22rpx;
			}
		}
		.notice{
			.notice_img{
				width:20.52rpx;
				height: 20.08rpx;
				right: 5.26%;
				left: 92%;
			}
		}
	}
	/*顶部用户信息*/
	.user-section {
		width: 750rpx;
		height: 392rpx;
		left: 0rpx;
		top: 0rpx;
		background: #FFAB4E;
		.middle{
			display: flex;
			padding-top:190rpx;
			padding-left:30rpx;
			.portrait_bg{
				// width: 100rpx;
				// height: 100rpx;
				// background: #FFFFFF;
				// border: 1px solid #FFFFFF;
				// border-radius: 50%;
				// .portrait{
				// 	width: 66rpx;
				// 	height: 66rpx;
				// 	margin:16rpx;
				// }
				.portrait{
					width: 100rpx;
					height: 100rpx;
					border-radius: 50%;
				}
			}
			.account-info{
				.username{
					display: flex;
					margin-left: 20rpx;
					margin-top: 22rpx;
					font-size: 24rpx;
					line-height: 28rpx;
					color: #FFFFFF;
					.sign{
						margin-left: 10rpx;
						margin-top: 4rpx;
						width:18.94rpx;
						height:18rpx;
					}
				}
				.account{
					margin-left: 20rpx;
					top:16rpx;
					font-size:18rpx;
					line-height: 22rpx;
					color: #888888;
				}
			}
			.arrow{
				.arrow-img{
					top: 26rpx;
					width:9rpx;
					height: 18rpx;
					// left:403.16rpx;
					left:390rpx;
				}
			}
	    }
		.bottom {
			display: flex;
			justify-content: space-around;
			padding-top:20rpx;
			.num {
				font-size: 28rpx;
				line-height: 34rpx;
				display: flex;
				justify-content: center;
				color: #FFFFFF;
			}
			.content{
				padding-top:10rpx;
				line-height: 28rpx;
				font-size: 24rpx;
				color: #585858;
			}
			// .tj-item{
			// 	margin-right:50rpx;
			// }
			// .tj-item-two{
			// 	margin-left:50rpx;
			// 	margin-right:50rpx;
			// }
			// .tj-item-four{
			// 	margin-left:50rpx;
			// }
		}
	}
	/*个人中心 内容区*/
	.user-content{
		margin-top: 10rpx;
		margin-left: 30rpx;
		/*我的订单*/
		.my-order{
			width: 690rpx;
			height: 216rpx;
			background: #FFFFFF;
			box-shadow: 0rpx 4rpx 8rpx rgba(142, 142, 142, 0.1);
			border-radius: 8rpx;
			.order-top{
				display: flex;
				padding-top: 20rpx;
				padding-bottom: 20rpx;
				border-bottom:2rpx solid #FFAB4E;
				.order-name{
					padding-left: 20rpx;
					font-size: 24rpx;
					line-height: 28rpx;
					color: #2D2D2D;
				}
				.look-order{
					margin-left: 424rpx;
					font-size: 20rpx;
					line-height: 24rpx;
					margin-bottom:2rpx;
					color: #888888;
				}
				.order-arrow-img{
					margin-left:10rpx;
					margin-top:6rpx;
					width:9rpx;
					height:18rpx;
				}
			}
			.order-center{
				display:flex;
				margin-left:2rpx;
				.nopay-img{
					height: 50rpx;
				}
				.order-content{
					padding-top:10rpx;
					font-size: 24rpx;
					line-height: 28rpx;
					color: #515151;
				}
				.order-center-item
				{
					margin-left:50rpx;
					margin-right:50rpx;
					width:72rpx;
					height:94rpx;
					margin-top:24rpx;
				}
			}
		}
		/*常用功能*/
		.common-function{
			width: 690rpx;
			height: 434rpx;
			margin-top: 30rpx;
			background: #FFFFFF;
			box-shadow: 0px 4rpx 8rpx rgba(142, 142, 142, 0.1);
			border-radius: 8rpx;
			.fct-top{
				padding-top: 20rpx;
				padding-bottom: 20rpx;
				border-bottom:2rpx solid #FFAB4E;
				.fct-name{
					padding-left: 20rpx;
					font-size: 24rpx;
					line-height: 28rpx;
					color: #2D2D2D;
				}
			}
			.fct-center{
				display: flex;
				flex-wrap: wrap;
				.fct-center-one,
				.fct-center-two,
				.fct-center-five,
				.fct-center-six{
					padding-left:38rpx;
					padding-right:38rpx;
				}
				.fct-center-three,
				.fct-center-seven{
					padding-left:38rpx;
					padding-right:40rpx;
				}
				.fct-center-four,
				.fct-center-eight{
					padding-left:40rpx;
					padding-right:18rpx;
				}
				.fct-center-all{
					padding-top:30rpx;
					padding-bottom:20rpx;
				}
				.address-img,.payment-img,
				.setting-img,.vip-img,
				.share-img,.service-img,
				.interview-img,.user-img{
					margin-left:8rpx;
					width:80rpx;
					height:80rpx;
				}
				.fct-content{
					font-size: 24rpx;
					line-height: 28rpx;
					display: flex;
					align-items: flex-end;
					color: #515151;
				}
				
			}
		}
		/*我的足迹*/
		.history-section {
			width: 690rpx;
			height: 350rpx;
			margin-top: 30rpx;
			background: #FFFFFF;
			box-shadow: 0px 4rpx 8rpx rgba(142, 142, 142, 0.1);
			border-radius: 8rpx;
			.fct-top{
				display: flex;
				padding-top: 20rpx;
				padding-bottom: 20rpx;
				border-bottom:2rpx solid #FFAB4E;
				.fct-name{
					padding-left: 20rpx;
					font-size: 24rpx;
					line-height: 28rpx;
					color: #2D2D2D;
				}
				.arrow-img{
					margin-left: 80%;
					margin-top:8rpx;
					width:9rpx;
					height:18rpx;
				}
			}
			.sec-header {
				display: flex;
				align-items: center;
				font-size: $font-base;
				color: $font-color-dark;
				.iconfont {
					color: #5eba8f;
					margin-right: 16upx;
					line-height: 40upx;
				}
				.content {
					flex: 1;
				}
				.iconyou {
					font-size: $font-base + 2upx;
					color: $font-color-base;
					margin-left: $uni-spacing-row-sm;
				}
			}
			.h-list {
				white-space: nowrap;
				padding: 20upx 30upx 0;
				.h-item {
					display: inline-block;
					font-size: $font-sm;
					color: $font-color-base;
					width: 160upx;
					margin-right: 20upx;
					border-radius: 10upx;
					.h-item-img {
						width: 160upx;
						height: 160upx;
					}
					.h-item-text {
						font-size: $font-sm;
					}
				}
			}

			.no-foot-print {
				text-align: center;
				padding: 48upx 0;

				.no-foot-print-icon {
					color: $base-color;
					font-size: $font-lg + 2upx;
					margin-right: 10upx;
				}
			}

			.share-btn {
				height: 102upx;
				text-align: left;
				background: none;
				padding: 0;
				margin: 0;
			}

			.share-btn:after {
				border: none;
				border-radius: none;
			}
		}
	}
	
}
</style>
