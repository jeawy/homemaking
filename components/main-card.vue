<template>
	<!-- 卡片 -->	
	<!-- <view> -->
		<!-- <view class="card_contant" > -->
			<view class="card"  @tap="navTo(`/pages/product/product?info=${info}`)">
				<!-- <view class="card_top" v-if="info.thumbnail_portait != null">
					<image class="card_img" :src="baseurl+info.thumbnail_portait?info.thumbnail_portait:imgsrc"></image>
				</view>				 -->
				<view class="card_top" >
					<image class="card_img" :src="imgsrc"></image>
				</view>				
				<view class="card_text">
					<view class="name">{{info.username}}</view>
					<view class="text_middle">
						<text class="middle-text">{{info.age}}岁</text>
						<image class="sex_img" src="../static/women.svg"></image>
						<text class="middle-text">{{info.nation}}</text>
						<span class="line">|</span>
						<text class="middle-text">从业{{info.workyears}}年</text>
					</view>
					<view class="language">
						<text class="language-text">{{info.languages}}</text>
						<span class="line">|</span>
						<text class="language-text">{{info.languages}}</text>
					</view>
				</view>
				<view class="card_bottom">
					<view class="bottom_left">{{info.category}}</view>
					<view class="bottom_button">
						<text class="button_text">预约面试</text>
					</view>
				</view>
			</view>
		<!-- </view> -->
</template>

<script>
	export default{
		props:[
			"info"
		],
		data(){
			return{
				baseurl:"",
				imgsrc:"/static/people.svg",
			}
		},
		methods:{
			//跳转详情
			navTo(route) {
				this.$mRouter.push({ route });
			},
		},
		onLoad() {
			this.baseurl = this.$mStore.state.BaseUrl
		}
	}
</script>

<style lang="scss" scoped>
	.font_text,.bottom_left,.button_text,.middle-text,.language-text{
		font-family: Tensentype MingSongJ-W4;
		font-size: 20rpx;
		font-style: normal;
		font-weight: 400;
		line-height: 24rpx;
		letter-spacing: 0em;
	}
	// .card_contant{
	// 	display: flex;
	// 	align-items: center;
	// 	width: 50%;
	// }
	.card{
		border: 2rpx solid #FFC107;
		box-sizing: border-box;
		border-radius: 16rpx;
		width: 344rpx;
		height: 504rpx;
		.card_top{
			width: 100%;
			padding: 24rpx 20rpx 0 18rpx;
			.card_img{				
				border-radius: 50%;
				width: 306rpx;
				height: 306rpx;
			}
		}		
		.card_text{
			display: block;
			justify-content: center;
			width: 100%;
			height: 92rpx;
			.name{
				text-align: center;
				font-family: Tensentype MingSongJ-W8;
				font-size: 24rpx;
				font-style: normal;
				font-weight: 900;
				line-height: 28.8rpx;
				letter-spacing: 0em;
			}
			.text_middle{
				line-height: 24rpx;
				margin: 12rpx 0;
				text-align: center;
				width: 100%;
				.sex_img{
					width: 12rpx;
					height: 19.58rpx;
					align-items: center;
					margin: 0 4rpx;
				}
				
			}
			.line{
				margin: 0 8rpx;
				height: 24rpx;
				line-height: 20rpx;
			}
			.language{
				line-height: 24rpx;
				text-align: center;
			}
		}
		.card_bottom{
			// width: 100%;
			display: flex;
			margin: 14rpx 40rpx 26rpx 40rpx;
			justify-content: center;
			align-items: center;
			.bottom_left{
				padding-right: 40rpx;
				color: rgba(255, 141, 14, 1);				
				text-align: center;
			}
			.bottom_button{
				width: 120rpx;
				height: 32rpx;				
				background: #FF8D0E;
				border-radius: 8rpx;
				display: flex;
				align-items: center;
				justify-content: center;				
				.button_text{
					color: #FFFFFF;					
					text-align: center;
				}
			}
		}
	}
</style>
