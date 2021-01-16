<template>
	<!-- 我的预约 -->
	<view class="my-appointment">
		<uni-calendar :insert="true" :lunar="true" :selected="selectedData" @change="dateChangeHandler"></uni-calendar>
		<view class="my-appointment_detail" v-if="detailData!=null">
			<view class="detail_title">
				{{detailData.date}}2012-12-24
			</view>
			<view class="detail_content" v-for="(item,index) of detailData.data" :key="index">
				<view class="content_time">
					{{item.time}}15:23
				</view>
				<view class="content_event">

					{{item.content}}213213
				</view>
			</view>
			<button class="synchroniz-btn" type="primary" @click="synchronizeToPhone">同步到手机日程</button>
		</view>
		<!-- W -->


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
				detailData: null,
				selectedData: []
			};
		},
		methods: {
			// 日历切换
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
			synchronizeToPhone() {
				// pass
			},
			// 请求预约列表
			queryList() {
				queryScheduleList().then(({
					status,
					msg
				}) => {
					if (status === 0) {
						const arr = msg.map(e => {
							return {
								date: dayjs(e.book_time * 1000).format('YYYY-MM-DD'),
								info: '预约',
								data: {
									id: e.id,
									name: e.aunty, //阿姨名称
									content: e.comment,
									time: dayjs(e.book_time * 1000).format('HH:mm'),
									phone: e.phone,
									status: e.status, // # 默认0 阿姨未查看 # 1 阿姨已同意 # 2 阿姨已拒绝
									producttype: e.producttype,
								}
							}
						})
						for (let e of arr) {
							// 如果同意日期已经添加过
							if (this.selectedData.findIndex((el) => e.data == el.data)) {
								continue
							}
							// 否则  相同日期的添加到一个data下面
							const SAME = arr.filter(el => {
								return el.data === e.data
							})
							this.selectedData.push({
								...e,
								data: SAME.map(s => s.data)
							})
						}
					}
				})
			}

		},
		onLoad() {
			this.queryList()
		}
	}
</script>

<style lang="scss">
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

				.content_time {
					border-right: 4upx solid #FFAB4E;
					margin-right: 16upx;
					padding: 8upx 16upx 8upx 0;
				}
			}
		}

		.synchroniz-btn {
			width: 375upx;
		}
	}
</style>
