<template>
	<view class="user">
		<!--头部-->
		<view class="user-section">
			<view class="middle">
				<!--用户头像-->
				<view class="portrait_bg">
					<image class="portrait"
						:src="user_portrait"></image>
				</view>
				<!--账户信息-->
				<view class="account-info">
					<view class="username">
						<text>
							{{username}}
						</text>
						<image class="sign"
							:src="account_sign"></image>
					</view>
					<text class="account">
						{{account}}
					</text>
				</view>
				<!--右箭头-->
				<view class="arrow">
					<image class="arrow-img" src="../../static/my/arrow_right_white.svg"></image>
				</view>
			</view>
			<!--关注的人-->
			<view class="bottom">
				<view class="tj-item-one">
					<text class="num">
						{{ focusernum }}
					</text>
					<text class="content">关注的人</text>
				</view>
				<view class="tj-item-two">
					<text class="num">
						{{ focuservicenum }}
					</text>
					<text class="content">关注服务</text>
				</view>
				<view class="tj-item-two">
					<text class="num">
						{{ footnum }}
					</text>
					<text class="content">足迹</text>
				</view>
				<view class="tj-item-four">
					<text class="num">
						{{ couponum }}
					</text>
					<text class="content">红包卡券</text>
				</view>
			</view>
			  
				 
		</view>
		<!-- 个人中心 内容区-->
		<view class="user-content">
			<!--我的订单-->
			<view class="my-order">
				<view class="order-top">
					<view class="order-name">我的订单</view>
					<view class="look-order">查看全部订单</view>
					<image class="order-arrow-img" src="../../static/my/arrow_right_orange.svg"></image>
				</view>
				<view class="order-center">
					<view class="order-center-one">
						<image class="nopay-img" src="../../static/my/nopay.svg"></image>
						<view class="order-content">待付款</view>
					</view>
					<view class="order-center-two">
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
					</view>
				</view>
			</view>
			<!--常用功能-->
			<view class="common-function">
				<view class="fct-top">
					<view class="fct-name">常用功能</view>
				</view>
				<view class="fct-center">
					<view class="fct-center-one fct-center-all">
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
					<view class="fct-center-six fct-center-all">
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
				</view>
			</view>
		</view>
	</view>
</template>
<script>
    export default {
        data() {
            return {
				message:"../../static/my/white_message.svg",
				setting:"../../static/my/set.svg",
				user_portrait_bg:"../../static/my/portrait_bg.svg",
				user_portrait:"../../static/my/portrait.svg",
				username:"测试男一号",
				account:"账号：user274859",
				account_sign:"../../static/my/king.svg",
				focusernum:2,
				focuservicenum:8,
				footnum:35,
				couponum:4
            }
        },
        methods: {
        }
    }
</script>
<style lang='scss' scoped>
.user {
	font-family: Tensentype MingSongJ-W2;
	font-style: normal;
	font-weight: normal;
	/*顶部用户信息*/
	.user-section {
		width: 750rpx;
		height: 392rpx;
		left: 0rpx;
		top: 0rpx;
		background: #FFAB4E;
		.top{
			left: 86.67%;
			top: 6.9%;
		}
		.middle{
			display: flex;
			padding-top:190rpx;
			padding-left:0px;
			.portrait_bg{
				width: 100rpx;
				height: 100rpx;
				margin-left: 30rpx;
				background: #FFFFFF;
				border: 1px solid #FFFFFF;
				border-radius: 50%;
				.portrait{
					width: 66rpx;
					height: 66rpx;
					margin:16rpx;
				}
			}
			.account-info{
				.username{
					display: flex;
					padding-left: 20rpx;
					padding-top: 22rpx;
					font-size: 24rpx;
					line-height: 28rpx;
					color: #FFFFFF;
					.sign{
						left: 10rpx;
						top: 4rpx;
						width:18.94rpx;
						height:18rpx;
					}
				}
				.account{
					padding-left: 20rpx;
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
					// left:403.16rpx
					left:390rpx
				}
			}
	    }
		.bottom {
			display: flex;
			padding-top:20rpx;
			padding-left:42rpx;
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
			.tj-item-one{
				margin-right:50rpx;
			}
			.tj-item-two{
				margin-left:50rpx;
				margin-right:50rpx;
			}
			.tj-item-four{
				margin-left:50rpx;
			}
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
				.nopay-img{
					height: 50rpx;
				}
				.order-content{
					padding-top:10rpx;
					font-size: 24rpx;
					line-height: 28rpx;
					color: #515151;
				}
				.order-center-one
				{
					margin-left:52rpx;
					margin-right:50rpx;
					width:72rpx;
					height:94rpx;
					margin-top:24rpx;
				}
				.order-center-two{
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
	}
}
</style>
