<template>
    <view class="swiper-item">
        <scroll-view
            class="list-scroll-content"
            scroll-y
        >
            <view class="order-list">
                <view
                    class="order-item"
                    v-for="(item, index) in orderList"
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
</template>

<script>
export default{
    props:[
        "orderList"
    ],
    data(){
        return{
        }
    },
    methods:{
        
    },
    onLoad() {
        console.log(orderList)
    }
}
</script>

<style lang="scss" scoped>
.uni-swiper-item{
	height: auto;
}
.list-scroll-content{
	height: 100vh;
	padding-top: 178rpx;//设置距离顶部的距离
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
</style>
