<template>
	<view class="content">
		<view class="g-header">
			<text>订单号：#{{ no }}</text>
			<text class="publish" @tap="openReviewEditor">发布评价</text>
		</view>
		<uniEvaluate :listData="listData" :rate="totalRate" />
		<view class="review-edit" v-if="showReviewEditor">
			<view class="top">
					<text class="cancel" @tap="()=>showReviewEditor=false">取消</text>
					<text>发表评价</text>
					<text class="publish" @tap="publishReview">确认</text>
			</view>
			<view>
				<view class="review-label">
					<text>订单号</text>
					<text>#{{ no }}</text>
				</view>
				<view class="stars-wrapper">
					<text>描述相符</text> 
					<view class="stars">
						<i v-for="star in stars" 
						:key="star.id" 
						:class="star.lit?'lit':''"
						class="iconfont iconstar"
						@tap="() => litStar(star.id)"></i>
					</view>
				</view>
				<textarea class="review-editor" 
						placeholder="请友善评价本次服务"
						v-model="review.content"></textarea>
			</view>
		</view>
	</view>
</template>

<script>
	import uniEvaluate from '../../components/xiujun-evaluate/uni-evaluate.vue';
	import store from '@/store'
	export default {
		components: {
			uniEvaluate
		},
		data() {
			return {
				no: null,
				listData: [
					{
						header_img: this.$mAssetsPath.headImg,
						user_name: "urmajesty516", 
						rate: 4,
						create_time: "2021-03-29",
						content: "awesome"
					},
				],
				stars: [
					{id: 1, lit: false},
					{id: 2, lit: false},
					{id: 3, lit: false},
					{id: 4, lit: false},
					{id: 5, lit: false}
				],
				review: {
					header_img: store.state.BaseUrl+store.state.userInfo.portrait,
					user_name: store.state.userInfo.username,
					rate: 0,
					create_time: "",
					content: ""
				},
				hasReview: false,
				showReviewEditor: false
			}
		},
		computed: {
			totalRate() {
				const total = this.listData.reduce((acc, curr) => 
					acc+curr.rate
				, 0)
				const avg = total/this.listData.length
				return Number(avg.toFixed(1))
			}
		},
		methods: {
			litStar(id) {
				this.stars.forEach(star => {
					if(star.id <= id) {
						star.lit = true
					}else{
						star.lit = false
					}	
				})
				this.review.rate = id
			},
			leadingZero(number) {
				return number < 10 ? "0"+number : number
			},
			openReviewEditor() {
				if(this.hasReview) {
					uni.showModal({
						content: "你已经评价过本次服务。",
						showCancel: false
					})
				}else{
					this.showReviewEditor = true
				}
			},
			publishReview() {
				console.log(store.state.userInfo)
				const now = new Date()
				const YY = now.getFullYear() 
				const MM = this.leadingZero(now.getMonth()+1)
				const DD = this.leadingZero(now.getDate())
				this.review.create_time = `${YY}-${MM}-${DD}`
				this.listData.push(this.review)
				this.hasReview = !this.hasReview
				this.showReviewEditor = false
			}
		},
		onLoad(options) {
			this.no = options.no
		}
	}
</script>

<style lang="scss" scoped>
	.content {
		background: $page-color-base;
		height: 100%;
		.g-header {
			display: flex;
			justify-content: space-between;
			align-items: center;
			background: #fff;
			padding: 30rpx;
			font-size: 30rpx;
			margin: 15rpx 0;
			.publish {
				color: #ff8d0e;
			}
		}
		.review-edit {
			position: absolute;
			bottom: 0;
			width: 100%;
			background: #fff;
			padding: 30rpx;
			.top {
				display: flex;
				font-size: 35rpx;
				justify-content: space-between;
				margin-bottom: 30rpx;
				.cancel {
					color: grey;
				}
				.publish {
					color: #ff8d0e;
				}
			}
			.review-label {
				display: flex;
				justify-content: space-between;
				font-size: 30rpx;
				padding: 15rpx 0;
				border-bottom: 1px solid #e6e6e6;
			}
			.stars-wrapper {
				display: flex;
				align-items: center;
				font-size: 30rpx;
				
				border-bottom: 1px solid #e6e6e6;
				.stars {
					margin-left: 30rpx;
					.iconstar {
						font-size: 50rpx;
						color:  #e6e6e6;
						&.lit {
							color: #fde16d;
						}
					}
				}
			}
			.review-editor {
				font-size: 30rpx;
				padding: 15rpx 0;
			}
		}
	}
</style>
