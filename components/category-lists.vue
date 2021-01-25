<template>
  <view class="category-list">
  	<view class="cate"> 
  		<image @tap="target('/pages/category/category', 27)"  class="cate_image_one" src="../static/home/maternity.svg" mode="aspectFill"></image>
  		<image @tap="target('/pages/category/category', 39)" class="cate_image_two" src="../static/home/babymeal.svg" mode="aspectFill"></image>
  		<image @tap="target('/pages/category/category', 38)" class="cate_image_three" src="../static/home/kids.svg" mode="aspectFill"></image>
  		<image @tap="target('/pages/category/category', 40)" class="cate_image_four" src="../static/home/elderly.svg" mode="aspectFill"></image>
  		<image @tap="target('/pages/category/category', 41)" class="cate_image_five" src="../static/home/housekeeping.svg" mode="aspectFill"></image>
  		<image @tap="target('/pages/category/category', 42)" class="cate_image_six" src="../static/home/hoursworker.svg" mode="aspectFill"></image>
  		<image @tap="target('/pages/category/category', 43)" class="cate_image_seven" src="../static/home/repairworker.svg" mode="aspectFill"></image>
  		<image @tap="target('/pages/category/category', 44)" class="cate_image_eight" src="../static/home/painworker.svg" mode="aspectFill"></image>
  		<image @tap="target('/pages/category/category', 45)" class="cate_image_nine" src="../static/home/moveworker.svg" mode="aspectFill"></image>
  		<image @tap="target('/pages/category/category', 46)" class="cate_image_ten" src="../static/home/lawnmaintainance.svg" mode="aspectFill"></image>
  	</view>		
  </view>
</template>
<script>
    export default{
		data(){
			return{
				
			}
		},
		methods:{
			target(url, id=0){
				if (id > 0){
			         uni.setStorageSync('categoryid',id);
				}
				this.$emit('target',url);				
			}
		}
	}
</script>
<style lang="less">
.category-list {
			height: 300rpx;
			margin: 0 40rpx;
			display: flex;
			flex-wrap: wrap;
			justify-content: space-around;
			background-color: #FFE8CD;
			border-radius: 16rpx;
			filter: drop-shadow(0px 2px 2px rgba(0,0,0,0.25));
			.cate{
				padding: 28rpx 36rpx 30rpx 38rpx;
				width: 100%;
				height: 300rpx;
				display: flex;
				flex-wrap: wrap;
					.cate_image_one{
						width: 100rpx;
						height: 102rpx;
					}
					.cate_image_two,
					.cate_image_three,.cate_image_four,
					.cate_image_five{
						width: 100rpx;
						height: 102rpx;
						margin-left: 24rpx;
					}
					.cate_image_seven,
					.cate_image_eight,.cate_image_nine,
					.cate_image_ten{
						width: 100rpx;
						height: 108rpx;
						margin-top: 32rpx;
						margin-left: 24rpx;
					}
					.cate_image_six{
						width: 100rpx;
						height: 108rpx;
						margin-top: 32rpx;
					}
					.cate_image_nine{
						height: 110rpx;
					}
			}
		}
</style>