<template>
	<view class="notice">
		<view class="listitem" v-for="(item, index) in noticelst" :key="index">
			<view class="detail" @tap="target('/pages/track/detail?id='+item.id)">
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
				<!-- <view class="group-btn">
					<view class="removeM btn-div" @tap="removeM(index, item.id)">
						删除
					</view>
				</view> -->
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
				noticelst: [{
					id: 1,
					src: '/static/notice/system.svg',
					time: '12:05',
					title: '系统通知',
					content: '续费会员马上领取价值250元红包',
					status: 0, //0:已读，1:未读
				}, {
					id: 2,
					src: '/static/notice/adsAnnoucement.svg',
					time: '16:05',
					title: '推送消息',
					content: '双11打折扣啦！',
					status: 0, //0:已读，1:未读
				}, {
					id: 3,
					src: '/static/notice/customerService.svg',
					time: '2020-10-19',
					title: '人工客服',
					content: '谢谢您的反馈',
					status: 1, //0:已读，1:未读
				}, {
					id: 4,
					src: '/static/people.svg',
					time: '2020-12-19',
					title: '张阿姨',
					content: '已接收转账',
					status: 1, //0:已读，1:未读
				}, {
					id: 5,
					src: '/static/people.svg',
					time: '2020-10-19',
					title: '小气',
					content: '好的,我等您通知',
					status: 1, //0:已读，1:未读
				}, {
					id: 4,
					src: '/static/people.svg',
					time: '2020-12-19',
					title: '张阿姨',
					content: '已接收转账',
					status: 0, //0:已读，1:未读
				}, {
					id: 5,
					src: '/static/people.svg',
					time: '2020-10-19',
					title: '艾伦',
					content: '好的,我等您通知',
					status: 1, //0:已读，1:未读
				}],

			};
		},
		methods: {

		},
		onLoad(option) {

		},
		computed: {

		},
	};
</script>
<style lang="scss" scoped>
.notice{
	background: #FFFFFF;
	padding: 28rpx 18rpx 10rpx 36rpx;
}
.listitem{
	margin-top: 20rpx;
	.detail,.right_top,.right_bottom{
		display: flex;
		align-items: center;
		
		width: 100%;
	}
	.detail{
		height: 100rpx;
	}
	.left{
		width: 80rpx;
		height: 80rpx;
		margin-bottom: 20rpx;
		.icon_dian{
			width: 80rpx;
			height: 80rpx;
			border-radius: 50%;
		}
	}
	.right{
		width: 100%;
		margin-left: 20rpx;
		padding-bottom: 26rpx;		
		border-bottom: 2rpx solid rgba(255, 141, 14, 0.35);
		.right_top{
			justify-content: space-between;
			margin-bottom: 16rpx;
			.title{				
				font-family: Tensentype MingSongJ-W4;
				font-size: 12px;
				font-style: normal;
				font-weight: 400;
				line-height: 14px;
				letter-spacing: 0em;
				text-align: left;
				color: #515151;

			}
			.time{
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
		.right_bottom{
			justify-content: space-between;
			.content{
				font-family: Tensentype MingSongJ-W4;
				font-size: 10px;
				font-style: normal;
				font-weight: 400;
				line-height: 12px;
				letter-spacing: 0em;
				text-align: left;
				color: #888888;

			}
			.red_circle{
				display: flex;
				justify-content: flex-end;
				&>view:nth-child(1){
					
					border-radius: 50%;
					width: 16rpx;
					height: 16rpx;
					background: #ff3b30;
				}
				
			}
		}
		
	}
}
</style>
