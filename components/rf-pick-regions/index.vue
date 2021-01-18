<template>
	<!-- 行政区划选择器 -->
	<picker v-if="multiArray.length" mode="multiSelector" @columnchange="columnChangeHandler" @change="valuecolumnChangeHandler"
	 :value="multiIndex" range-key="short_name" :range="multiArray">
		{{multiStr}}
	</picker>
</template>

<script>
	import {
		provinceList,
	} from '@/api/basic';
	import {
		queryProvinceList,
		queryCityList,
		queryCountyList
	} from '@/api/userInfo.js';
	export default {
		// 地区id 双向绑定
		props: ['areaCode'],
		data() {
			return {
				multiArray: [],
				multiIndex: [],
				multiStr: ''
			}
		},
		created() {
			this.getProvinceList();
		},
		methods: {
			valuecolumnChangeHandler({
				detail: {
					value
				}
			}) {
				this.multiStr =
					`${this.multiArray[0][value[0]].short_name} / ${this.multiArray[1][value[1]].short_name} / ${this.multiArray[2][value[2]].short_name}`
				this.$emit('update:area-code', this.multiArray[2][value[2]].id)
			},
			// 请求默认值
			getProvinceList() {
				queryProvinceList().then(list => {
					this.multiArray.splice(0, 1, list)
					this.multiIndex.splice(0, 1, 0)
					return queryCityList({
						provinceid: list[0].id
					})
				}).then((list) => {
					this.multiArray.splice(1, 1, list)
					this.multiIndex.splice(1, 1, 0)
					return queryCountyList({
						cityid: list[0].id
					})
				}).then((list) => {
					this.multiArray.splice(2, 1, list)
					this.multiIndex.splice(2, 1, 0)
				})
			},
			columnChangeHandler({
				detail: {
					column,
					value
				}
			}) {
				this.multiIndex.splice(column, 1, value)
				// 区县change 不请求后台
				if (column === 2) {
					return false
				}
				// 改变时候由省ID请求市
				if (column === 0) {
					queryCityList({
						provinceid: this.multiArray[column][value].id
					}).then((list) => {
						this.multiArray.splice(1, 1, list)
						return queryCountyList({
							cityid: list[0].id
						})
					}).then((list) => {
						this.multiArray.splice(2, 1, list)
					})
					return false
				}
				// 否则市ID请求县区
				queryCountyList({
					cityid: this.multiArray[column][value].id
				}).then((list) => {
					this.multiArray.splice(2, 1, list)
				})

			}
		}
	}
</script>
