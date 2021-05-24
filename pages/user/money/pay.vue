<template>
	<view class="app">
		<view class="price-box">
			<text>支付金额</text>
			<text class="price">{{ money }}</text>
		</view>
		<view class="pay-type-list">
			<view class="type-item b-b" @tap="changePayType(1)"  >
				<i class="iconfont icon iconweixinzhifu"></i>
				<view class="con">
					<text class="tit">微信支付</text>
					<text>推荐使用微信支付</text>
				</view>
				<label class="radio">
					<radio value="" color="#fe8e2e" :checked='payType == 1'/>
				</label>
			</view>
			<view class="type-item b-b" @tap="changePayType(2)" >
				<i class="iconfont icon iconalipay"></i>
				<view class="con">
					<text class="tit">支付宝支付</text>
					<text>推荐使用支付宝支付</text>
				</view>
				<label class="radio">
					<radio value="" color="#fe8e2e" :checked='payType == 2'/>
				</label>
			</view>
			<view class="type-item b-b" @tap="changePayType(4)"  >
				<!-- <i class="iconfont icon iconweixinzhifu"></i> -->
				<img src="static/creditCard.png" width="30" :style="{paddingRight: '40rpx'}" />
				<view class="con">
					<text class="tit">信用卡支付</text>
					<text>推荐使用MasterCard或Visa</text>
				</view>
				<label class="radio">
					<radio value="" color="#fe8e2e" :checked='payType == 4'/>
				</label>
			</view>
			<view class="type-item" @tap="changePayType(5)" v-if="parseInt(payTypeList.order_balance_pay, 10) === 1">
				<i class="iconfont icon iconerjiye-yucunkuan"></i>
				<view class="con">
					<text class="tit">预存款支付</text>
					<text>可用余额 {{ userInfo && userInfo.account && userInfo.account.user_money }}</text>
				</view>
				<label class="radio">
					<radio value="" color="#fe8e2e" :checked='payType === 5'/>
				</label>
			</view>
		</view>
		<button class="confirm-btn" :disabled="btnLoading" :loading="btnLoading" @tap="confirm">确认支付</button>
	</view>
</template>
//<script src="https://js.stripe.com/v3/"></script>
<script>
	import store from '@/store'
    import {orderPay} from "@/api/product";
    import {orderDetail, paystripe} from "@/api/userInfo";
    import {configList} from "@/api/basic";
    export default {
        data() {
            return {
                payType: 5,
                payTypeList: {},
                money: 0,
                userInfo: {},
                orderDetail: {},
				orderInfo: {},
				billinfo: {},
                btnLoading: false
            };
        },
        computed: {},
        onLoad(options) {
            this.initData(options);
        },
        methods: {
            // 数据初始化
            async initData(options) {
                this.orderInfo.order_id = parseInt(options.id, 10);
                 
                this.getOrderDetail(options.id);
                this.userInfo = uni.getStorageSync('userInfo') || undefined;
                // #ifdef H5
		            if (uni.getSystemInfoSync().platform === 'android') await this.$mPayment.wxConfigH5();
				        // #endif
            },
            //选择支付方式
           changePayType(type) {
                this.payType = type;
            },
            // 获取订单费用
            async getOrderDetail(id) {
                await this.$http.get(`${orderDetail}`, {
                    id:this.orderInfo.order_id, // 订单id
                    simplify: 1 // 获取简化订单详情
                }).then(r => {
					
					console.log(r)
					this.billinfo = r.msg
                    this.money = r.msg.money
                });
            },
            //确认支付
            confirm() {
                this.btnLoading = true;
				console.log(this.payType)
                switch (parseInt(this.payType)) {
                  case 1:
                    this.$mPayment.weixinPay('order', JSON.stringify(this.billinfo));
                    break;
                  case 2:
                    this.$mPayment.aliPay('order',  this.billinfo);
                    break;
				  case 4:
					//信用卡支付 
					
					// 这里原来是 store.state.estateToken || uni.getStorageSync('estateToken')
					var token = store.state.accessToken || uni.getStorageSync('accessToken');
					console.log('login token: ', token)

					uni.navigateTo({
					//   url: 'stripe?id=' + this.orderInfo.order_id + '&isLogin='+token,
					  url: 'stripe?no=' + this.billinfo.no + '&isLogin='+token,
					  events: {
					    // 为指定事件添加一个监听器，获取被打开页面传送到当前页面的数据
					    acceptDataFromOpenedPage: function(data) {
					      console.log(data)
					    },
					    someEvent: function(data) {
					      console.log(data)
					    }
					     
					  },
					  success: function(res) {
					    // 通过eventChannel向被打开页面传送数据
						console.log("dddd")
					    res.eventChannel.emit('acceptDataFromOpenerPage', { data: 'test' })
					  },
					  fail:function(res){
						  console.log(res)
					  }
					})
					break
                  case 5:
                    this.$mPayment.balancePay(JSON.stringify(this.billinfo));
                    break;
                }
                setTimeout(() => {
                	this.btnLoading = false;
                }, 0.5 * 1000)
            }
        }
    }
</script>

<style lang='scss'>
	page {
		background-color: $color-white;
	}

	.app {
		width: 100%;
	}

	.price-box {
		background-color: #fff;
		height: 265upx;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		font-size: 28upx;
		color: #909399;

		.price {
			font-size: 50upx;
			color: #303133;
			margin-top: 12upx;

			&:before {
				content: '￥';
				font-size: 40upx;
			}
		}
	}

	.pay-type-list {
		margin-top: 20upx;
		background-color: #fff;
		padding-left: 60upx;

		.type-item {
			height: 120upx;
			padding: 20upx 0;
			display: flex;
			justify-content: space-between;
			align-items: center;
			padding-right: 60upx;
			font-size: 30upx;
			position: relative;
		}

		.icon {
			width: 100upx;
			font-size: 52upx;
		}

		.iconerjiye-yucunkuan {
			color: #fe8e2e;
		}

		.iconweixinzhifu {
			color: #36cb59;
		}

		.iconalipay {
			color: #01aaef;
		}

		.tit {
			font-size: $font-lg;
			color: $font-color-dark;
			margin-bottom: 4upx;
		}

		.con {
			flex: 1;
			display: flex;
			flex-direction: column;
			font-size: $font-sm;
			color: $font-color-light;
		}
	}

</style>
