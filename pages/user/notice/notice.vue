<template>
	<view class="notice">
		<view class="listitem" 
			v-for="(item, index) in noticelst" 
			:key="index" 
			@touchstart="touchSlideStart($event,index)"
			 @touchend="touchSlideEnd($event,index)" 
			 @touchmove="touchSlideMove($event,index)" 
			 :style="{transform:'translate3d('+item.slide_x+'px, 0, 0)'}">
			<view class="detail" :style="{minWidth: screenWidth+'px'}" @tap="target('/pages/track/detail?id='+item.id)">
				<view class="left">
					<image class="icon_dian" :src='item.src'></image>
				</view>
				<view class="right">
					<view class="right_top">
						<view class="title">{{item.title}}</view>
						<view class="time">{{item.time}}</view>

					</view>
					<view class="right_bottom">
						<view class="content">{{item.content}}</view>
						<view class="red_circle" v-if="item.status==0">
							<!-- 红色圆点 ：表示未读-->
							<view></view>
						</view>

					</view>
				</view>
			</view>
			<view class="group-btn">
				<view class="removeM btn-div" @tap="removeM(index, item.id)">
					删除
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		components: {

		},
		data() {
			return {
				screenWidth: '',
				noticelst: [{
					id: 1,
					src: '/static/notice/system.svg',
					time: '12:05',
					title: '系统通知',
					content: '续费会员马上领取价值250元红包',
					status: 0, //0:已读，1:未读
					slide_x: 0 //列表左滑距离
				}, {
					id: 2,
					src: '/static/notice/adsAnnoucement.svg',
					time: '16:05',
					title: '推送消息',
					content: '双11打折扣啦！',
					status: 0, //0:已读，1:未读
					slide_x: 0 //列表左滑距离
				}, {
					id: 3,
					src: '/static/notice/customerService.svg',
					time: '2020-10-19',
					title: '人工客服',
					content: '谢谢您的反馈',
					status: 1, //0:已读，1:未读
					slide_x: 0 //列表左滑距离
				}, {
					id: 4,
					src: '/static/aunty.png',
					time: '2020-12-19',
					title: '张阿姨',
					content: '已接收转账',
					status: 1, //0:已读，1:未读
					slide_x: 0 //列表左滑距离
				}, {
					id: 5,
					src: '/static/aunty.png',
					time: '2020-10-19',
					title: '小气',
					content: '好的,我等您通知',
					status: 1, //0:已读，1:未读
					slide_x: 0 //列表左滑距离
				}, {
					id: 4,
					src: '/static/aunty.png',
					time: '2020-12-19',
					title: '张阿姨',
					content: '已接收转账',
					status: 0, //0:已读，1:未读
					slide_x: 0 //列表左滑距离
				}, {
					id: 5,
					src: '/static/aunty.png',
					time: '2020-10-19',
					title: '艾伦',
					content: '好的,我等您通知',
					status: 1, //0:已读，1:未读
					slide_x: 0 //列表左滑距离
				}],
				start_slide_x: 0,
				btnWidth: 0,
				startX: 0,
				LastX: 0,
				startTime: 0,
			};
		},
		methods: {
			// 滑动开始
			touchSlideStart(e, index) {
				// console.log(index)
				//记录手指放上去的时间
				this.startTime = e.timeStamp;
				//记录滑块的初始位置
				this.start_slide_x = this.noticelst[index].slide_x;

				// 按钮宽度
				uni.createSelectorQuery()
					.selectAll('.group-btn')
					.boundingClientRect()
					.exec(res => {
						if (res[0] != null) {
							this.btnWidth = res[0][index].width * -1;
						}
					});
				// 记录上一次开始时手指所处位置
				this.startX = e.touches[0].pageX;
				// 记录上一次手指位置
				this.lastX = this.startX;
				//初始化非当前滑动消息列的位置
				if(this.tracks){
				this.tracks.forEach((item, eq) => {
					if (eq !== index) {
						item.slide_x = 0;
					}
				});
					};
			},
			// 滑动中
			touchSlideMove(e, index) {

				const endX = e.touches[0].pageX;
				const distance = endX - this.lastX;
				// 预测滑块所处位置
				const duang = this.noticelst[index].slide_x + distance;
				// 如果在可行区域内

				if (duang <= 0 && duang >= this.btnWidth) {
					this.noticelst[index].slide_x = duang;
				}

				// 此处手指所处位置将成为下次手指移动时的上一次位置
				this.lastX = endX;


			},
			// 滑动结束
			touchSlideEnd(e, index) {

				let distance = 10;
				const endTime = e.timeStamp;
				const x_end_distance = this.startX - this.lastX;
				if (Math.abs(endTime - this.startTime) > 200) {
					distance = this.btnWidth / -2;
				}
				// 判断手指最终位置与手指开始位置的位置差距
				if (x_end_distance > distance) {
					this.noticelst[index].slide_x = this.btnWidth;
				} else if (x_end_distance < distance * -1) {
					this.noticelst[index].slide_x = 0;
				} else {
					this.noticelst[index].slide_x = this.start_slide_x;
				}

			},
			// 删除
			removeM(index, id) {
				let self = this
				uni.showModal({
					title: '',
					content: '确定要删除该信息吗？',
					confirmText: '删除',
					confirmColor: '#ff3b32',
					success: function(res) {
						if (res.confirm) {
							let track = self.noticelst.splice(index, 1)
							const data = {  
							method: 'delete',
							id : track[0].id,
							}
							 postLocation(data).then((res)=>{
								if (res.data.status == 0){
								self.floterText = "已加载全部，共(" + self.tracks.length + ")条轨迹"
							 	}
								else{console.log(res)}

							 });
							 uni.showToast({
							 	icon: "success",
								title: '删除成功!',
							 	duration: 2000
							});
						} else if (res.cancel) {
							console.log('用户点击取消')
						}
					}
				});
			},
		},
		onLoad(option) {

		},
		onReady() {
			let system_info = uni.getSystemInfoSync();
			this.screenWidth = system_info.screenWidth-30
		},
		computed: {

		},
	};
</script>
<style lang="scss" scoped>
	.notice {
		background: #FFFFFF;
		padding: 28rpx 18rpx 10rpx 36rpx;
	}

	.listitem {
		display: flex;
		margin-top: 20rpx;

		.right_top,
		.right_bottom {
			display: flex;
			align-items: center;
			// width: 100%;
		}

		.detail {
			// min-width: 650rpx;
			display: inline-flex;
			height: 100rpx;
		}

		.left {
			width: 80rpx;
			height: 80rpx;
			margin-bottom: 20rpx;

			.icon_dian {
				width: 80rpx;
				height: 80rpx;
				border-radius: 50%;
			}
		}

		.right {
			width: 100%;
			margin-left: 20rpx;
			padding-bottom: 26rpx;
			border-bottom: 2rpx solid rgba(255, 141, 14, 0.35);

			.right_top {
				justify-content: space-between;
				margin-bottom: 16rpx;

				.title {
					font-family: Tensentype MingSongJ-W4;
					font-size: 12px;
					font-style: normal;
					font-weight: 400;
					line-height: 14px;
					letter-spacing: 0em;
					text-align: left;
					color: #515151;

				}

				.time {
					font-family: Tensentype MingSongJ-W4;
					font-size: 10px;
					font-style: normal;
					font-weight: 400;
					line-height: 12px;
					letter-spacing: 0em;
					text-align: center;
					color: #888888;
				}

			}

			.right_bottom {
				justify-content: space-between;

				.content {
					font-family: Tensentype MingSongJ-W4;
					font-size: 10px;
					font-style: normal;
					font-weight: 400;
					line-height: 12px;
					letter-spacing: 0em;
					text-align: left;
					color: #888888;

				}

				.red_circle {
					display: flex;
					justify-content: flex-end;

					&>view:nth-child(1) {

						border-radius: 50%;
						width: 16rpx;
						height: 16rpx;
						background: #ff3b30;
					}

				}
			}

		}

		.group-btn {
			float: left;
			display: flex;
			flex-direction: row;
			min-width: 220rpx;
			margin-left: 40rpx;
			align-items: center;
		}

		.group-btn .btn-div {
			height: 100%;
			min-width: 220rpx;
			color: #fff;
			text-align: center;
			font-size: 34upx;
			line-height: 100rpx;
		}

		.group-btn .removeM {
			background-color: #FFA23A;
		}
	}
</style>
