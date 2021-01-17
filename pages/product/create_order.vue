<template>
	<view class="create-orders">
		<view class="order-address">
			请选择收获地址&nbsp;&nbsp;>
		</view>
		<view>
			<text>商品列表</text>
		</view>
		<view class="goods-container">
			<image class="goods-img " src="../../static/emoji/10.png"></image>
			<view class="goods-info">
				<view>
					<text>商品名称</text>
				</view>
				<view>
					<text>商品规格*数量</text>
				</view>
				<view>
					<text>商品总价格</text>
				</view>
			</view>
		</view>
		<!--
		<view>
			<text>商品金额</text>
		</view>
		<view>
			<text>商品数量</text>
		</view>
		-->
		<view>
			<view>
				备注<input class="uni-input" placeholder="请填写备注信息" />
			</view>
		</view>
	</view>
</template>

<script>
</script>

<style lang="scss" scoped>
	.create-orders{
		padding-top: 20upx;
		.goods-container{
			display: flex;
			padding: 40upx 0 20upx;
			.goods-img{
				width: 200upx;
				height: 200upx;
				border-radius: 6upx;
				//border: 4upx solid white;
				flex: none;
			}
			.goods-info{
				margin-left: 20upx;
			}
		}
	}
</style>
