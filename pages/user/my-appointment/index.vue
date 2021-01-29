<template>
	<!-- 我的预约 -->
	<view class="my-appointment">

		<!-- 日历组件 -->
		<uni-calendar ref="calendar" :insert="true" :lunar="true" :selected="selectedData" @change="dateChangeHandler"></uni-calendar>

		<!-- 预约详情展示 -->
		<view class="my-appointment_detail" v-if="detailData!=null">
			<view class="detail_title">
				{{detailData.date}}
			</view>
			<view class="detail_content" v-for="(item,index) of detailData.data" :key="index">
				<view class="content_time">
					{{item.time}}
				</view>
				<view class="view_vedio" @tap="navTo('/pages/public/vedio')" >
					<image class="sex_img" src="/static/vedio.svg"></image>
				</view>
				<view class="content_event">
					{{item.content}}
				</view>
			</view>
			
		</view>
		<!--
		<button class="synchroniz-btn" :disabled="detailData==null" type="primary" @click="syncToPhone">同步到手机日程</button>
		-->
	</view>
</template>

<script>
	import uniCalendar from '@/components/uni-calendar/uni-calendar.vue'
	import dayjs from "dayjs"
	import {
		queryScheduleList
	} from "@/api/userInfo.js"
	export default {
		components: {
			uniCalendar
		},
		data() {
			return {
				// 预约详情
				detailData: null,
				// 预约列表
				selectedData: [],
			};
		},
		methods: {
			// 日历切换展示选择日期下的预约
			dateChangeHandler({
				extraInfo
			}) {
				if (!Object.keys(extraInfo).length) {
					this.detailData = null
					return
				}
				this.detailData = extraInfo
			},
			// 同步到手机
			syncToPhone() {
				// pass
			},
			// 统一跳转接口,拦截未登录路由
            navTo(route) {
				//console.log(route)
                if (!route) {
                    return;
				}
				console.log(route)
                
                this.$mRouter.push({route});
                
            },
			// 请求预约列表
			queryList() {
				return queryScheduleList().then(({
					status,
					msg
				}) => {
					if (status === 0) {
						const ARR = msg.map(e => {
							return {
								date: dayjs(e.book_time * 1000).format('YYYY-MM-DD'),
								info: '预约',
								data: {
									id: e.id,
									name: e.aunty,
									content: e.comment,
									time: dayjs(e.book_time * 1000).format('HH:mm'),
									phone: e.phone,
									status: e.status, // # 默认0 阿姨未查看 # 1 阿姨已同意 # 2 阿姨已拒绝
									producttype: e.producttype,
								}
							}
						})
						// 日历组件同一天只显示列表中的一项，所以要将同一天的预约合并到一项数据的data列表中
						for (let item of ARR) {
							// 如果同一日期已经添加过
							if (this.selectedData.findIndex((el) => item.date == el.date) > -1) {
								continue
							}
							// 否则  相同日期的添加到一个data下面
							const SAME = ARR.filter(el => {
								return el.date === item.date
							})
							// 组合成预约列表
							this.selectedData.push({
								...item,
								data: SAME.map(s => s.data)
							})
						}
					}
				})
			}

		},
		async onReady () {
			await this.queryList()
			// 初始化获取当天的预约详情
			const today = dayjs(new Date()).format('YYYY-MM-DD')
			this.detailData = this.selectedData.find((e)=>e.date==today) 
		}
	}
</script>

<style lang="scss" scoped>

	.my-appointment {
		display: flex;
		flex-flow: column nowrap;
		min-height: calc(100vh - 88upx);
	
        
		// .my-appointment_detail__nothing {
		// 	padding: 16upx 32upx;
		// 	color: #666;
		// 	margin-top: 20upx;
		// 	background-color: #fff;
		// }

		.my-appointment_detail {
			background-color: #fff;
			flex: 1 0 auto;
			padding: 16upx;
			margin-top: 20upx;

			.detail_title {
				padding: 16upx 0;
				font-weight: 600;
			}

			.detail_content {

				padding: 16upx;
				display: flex;
				align-items: center;
				border-bottom: 2upx solid #DDDDDD;
                .sex_img{
					height: 40px;
					width: 80px; 
					border-right: 4upx solid #FFAB4E;
					margin-top:7px;
				}
				.view_vedio{
					width: 100px; 
					margin-right: 16upx;
					padding: 8upx 16upx 8upx 0;
				}
				.content_time {
					height: 40px;
					border-right: 4upx solid #FFAB4E;
					margin-right: 16upx;
					padding: 16upx 16upx 8upx 0;
				}
			}
		}

		.synchroniz-btn {
			position: fixed;
			bottom: 0;
			left: 50%;
			transform: translateX(-50%);
			width: 375upx;
		}
	}
</style>
