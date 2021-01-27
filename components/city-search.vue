<template>
	<view>
		<view class="location" @tap="positionSearch">
			<view class="location_text">{{city}}</view>
			<image class="location_img" src="/static/home/City.svg"></image>		
		</view>
		<s-popup custom-class="demo-popup" position="center" v-model="cityVisiable">
			<view class="position_search">
			    <view class="position_box">
			        <view class="position_title">请选择服务地址</view>
			        <view class="titlename">当前城市</view>
			        <view class="box_image">
			            <view class="box_title">
			                <view class="box_name">{{city}}</view>
			                <image class="position_img" src="/static/home/Position.svg"></image>
			            </view>
			            <image class="refresh_img" src="/static/home/Refresh.svg" @click="refresh()"></image>
			        </view>
			    </view>
			    <view class="value">
			        <view class="value_name">热门城市</view>
			        <view class="cityList">
			            <view class="city_text" v-for="(item,index) in citylist" 
			            :key="index">{{item.value}}</view>
			        </view>
			    </view>
			</view>
		</s-popup>
	</view>
</template>
<script>
	import sPopup from '@/components/s-popup/index.vue';
    export default{
		props:['city'],
		data(){
			return{
				cityVisiable:false,
				citylist:[
					{value:'悉尼'},
					{value:'墨尔本'},
					{value:'布里斯班'},
					{value:'柏斯'},
					{value:'阿德莱德'},
					{value:'黄金海岸'},
					{value:'卧龙岗'},
					{value:'堪培拉'},
					{value:'基隆'},
					{value:'霍巴德'},
					{value:'达尔文'},
					{value:'洛坎普顿'},		
					]
			}
		},
		components:{
			sPopup
		},
		methods:{
			refresh(){
				this.$emit("doGetLocation");
				this.cityVisiable = !this.cityVisiable;
			},
			positionSearch(){
				this.cityVisiable = !this.cityVisiable;
			}
		}
	}
</script>
<style lang="less">
	// 定位
	.location{	
		display: flex;
		align-items: center;
		.location_text{
			margin-left: 40rpx;
			font-family: Tensentype MingSongJ-W2;
			font-size: 24rpx;
			line-height: 28rpx;
		}
		.location_img{
			margin-left: 4rpx;
			width: 16rpx;
			height: 24rpx;
		}
	}
    .position_search{
        width: 560rpx;		
        .box_image{
            display: flex;
            align-items: center;
        }
        .position_box{
            // height: 114rpx;
            .position_title{
                text-align: center;
                margin-top: 10rpx;
                margin-bottom: 8rpx;
                font-family: Tensentype MingSongJ-W2;
                font-size: 20rpx;
                line-height: 24rpx;				
            }
            .titlename{
                font-family: Tensentype MingSongJ-W2;
                font-size: 18rpx;
                line-height: 22rpx;
                margin-left: 10rpx;
            }
        }
        .box_image{	
            justify-content: space-between;
            margin-top: 8rpx;
            margin-bottom: 20rpx;
            .box_title{
                display: flex;
                margin-left: 20rpx ;					
                align-items: center;
                .box_name{
                    font-family: Tensentype MingSongJ-W8;
                    font-size: 18rpx;
                    // font-style: normal;
                    font-weight: 900;
                    line-height: 22rpx;
                    // letter-spacing: 0em;						
                }
                .position_img{
                    width: 9.17px;
                    height: 9.17px;
                    margin-left: 3.42px;					
                }					
            }
            .refresh_img{
                width: 8.27px;
                height: 8.26px;
                margin-right: 11.36px;
            }
        }						
        .value{
            background: #fff7ee;
            height: 202rpx;
            border-radius: 8rpx;
            .value_name{
                font-family: Tensentype MingSongJ-W2;
                font-size: 18rpx;
                line-height: 22rpx;
                margin: 0 0 20rpx 10rpx;
                padding-top: 6rpx;
            }
            .cityList{
                display: flex;
                flex-wrap: wrap;
                align-items: center;
                .city_text{
                    display: flex;
                    align-items: center;
                    justify-content: center;
                    width: 100rpx;
                    height: 32rpx;
                    background: #FDB667;
                    border-radius: 4rpx;
                    margin: 0 20rpx 20rpx 20rpx;
                    font-family: Tensentype MingSongJ-W4;
                    font-size: 18rpx;
                    // font-style: normal;
                    // font-weight: 400;
                    line-height: 22rpx;
                    // letter-spacing: 0em;
                    text-align: center;
                    color: #ffffff;

                }
            }
        }
    }
	.demo-popup{
		.s-popup-wrap{
			height: 316rpx;
		}
	}
</style>