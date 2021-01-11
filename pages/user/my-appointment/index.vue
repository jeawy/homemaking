<template>
	<!-- 我的预约 -->
	<view class="my-appointment">
		<uni-calendar :insert="true" :lunar="true" :selected="selectedData" @change="dateChangeHandler"></uni-calendar>
		<view class="my-appointment_etaile" v-if="detailData!=null">
			{{detailData.data.name}}
			<br>
			{{detailData.data.content}}
			<br>
			{{detailData.date}} {{detailData.data.time}}
		</view>
		<view class="my-appointment_etaile__nothing" v-else>
			无预约
		</view>
		<button class="synchroniz-btn" type="primary" @click="synchronizeToPhone">同步到手机日程</button>
	</view>
</template>

<script>
	import uniCalendar from '@/components/uni-calendar/uni-calendar.vue'
	export default {
		components: {
			uniCalendar
		},
		data() {
			return {
				detailData: null,
				selectedData: [{
					date: '2021-01-12',
					info: '预约',
					data: {
						name: '名称啊哈',
						content: '预约了个寂寞',
						time: '12:20',
						rows: [{
							a: 1,
							b: 2
						}]
					}
				}, {
					date: '2021-01-14',
					info: '预约',
					data: {
						custom: '自定义信息',
						name: '自定义消息头',
						time: '12:20',
						rows: [{
							a: 4444,
							b: 2
						}]
					}
				}]
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
			}
		}
	}
</script>

<style lang="scss">
	.my-appointment {
		.my-appointment_etaile {}
		.synchroniz-btn{
			width: 375upx;
		}
	}
</style>
