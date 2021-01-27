<template>
	<view class="interview">
		<view class="top_line"></view>
		<!-- W -->
		<form class="form" @submit="formSubmit">
			<view class="list_name">
				预约的阿姨:{{personInfo.productname}}
			</view>
			
			<view class="list_dept schedule-section">
				<image class="uni-img" src="../../static/houseKeeping/people.png"></image>
				<view class="line">
					{{personInfo.category}}
				</view>
				<!-- <view v-if="deptarray.length == 0" class="line">
					<input class="uni-input" name="input" :disabled='true' placeholder="请选择工种" />
				</view>
				<view v-else class="line">
					<picker class="picker" mode="selector" @change="bindPickerdept" :value="deptindex" :range="deptarray">
						<view class="uni-dept-input">{{deptarray[deptindex]}}</view>
					</picker>
				</view> -->
			</view>
			<view class="list_time schedule-section">
				<image class="uni-img" src="../../static/houseKeeping/time.png"> </image>
				<view class="line">
					<picker class="picker" mode="date" :value="form.date" @change="bindTimeChange($event,'date')">
						<view class="uni-time-input">{{form.date||'选择日期'}}</view>
					</picker>
					<picker class="picker" mode="time" :value="form.time" @change="bindTimeChange($event,'time')">
						<view class="uni-time-input">{{form.time||'选择时间'}}</view>
					</picker>
				</view>
			</view>
			<view class="list_textarea schedule-section">
				<textarea class="textarea_content" v-model="form.comment" placeholder-style="color:#9e9e9e" placeholder="请输入您的服务内容" />
				</view>
			<view class="action-btn">
				<view :loading="isLoading" @tap="formSubmit()"  class="action-btn no-border buy-now-btn">立即预约</view>
			</view>
		</form>
	</view>
</template>

<script>
	import {postSchedule} from "@/api/product.js"
	import dayjs from "dayjs"
	export default{
		data(){
			return{
				personInfo:{},
				form:{date:'',time:'', comment:'预约面试'},
				isLoading:false,
				deptarray:['清洁工', '洗碗工'],
				deptindex:0,
				addreindex:0,
			}
		},
		onLoad(e) {
			this.personInfo = uni.getStorageSync('product_detail',this.productDetail)
			uni.setStorageSync('product_detail',undefined)
		},
		methods:{
			bindTimeChange: function(e,key) {
				console.log(key,e)
				this.form[key] = e.detail.value
			},
			formSubmit: function(e){
				if(this.form.date === ''){
					this.$mHelper.toast('请选择日期');
					return;
				}
				if(this.form.time === ''){
					this.$mHelper.toast('请选择时间');
					return;
				}
				this.isLoading = true
				const data = {
					comment:this.form.comment,
					book_time:`${this.form.date.replace(/\-/g,'/')} ${this.form.time}`,
					productid:this.personInfo.id
				}
				postSchedule(data).then(({status,msg})=>{
					this.$mHelper.toast(msg);
					if(status == 0){
						uni.navigateBack()
					}
				})
				.catch((err)=>{
					this.$mHelper.toast(err);
				})
				.finally(()=>{
					this.isLoading = false
				})
				
			}
		}
	}
</script>

<style lang="scss" scoped>
	.interview{
		background-color: #FFFFFF;
		.top_line{
			width: 100%;
			height: 2rpx;
			background-color: #F1F1F1;
		}
		.schedule-section{
			display: flex;
			.uni-img{
				width: 60rpx;
				height: 60rpx;
				flex: 0 0 60rpx;
			}
		}
	}
	
	.picker{
		width: 100%;
	}
	.list_name{
		border-bottom: 2rpx solid #f1f1f1;
	}
	.list_name,.list_people,.list_phone,.list_dept,.list_time,.list_address{
		display:flex;
		align-items: center;
		padding: 32rpx;				
	}
	.line,.uni-input-people,.uni-input-phone{
		display: flex;
		align-items: center;
		width: 100%;
		height: 100rpx;
		margin-left: 25rpx;
		border-bottom: 2rpx solid #f1f1f1;
	}
	.uni-input-people,.uni-input-phone{
		
	}
	.uni-input,.uni-dept-input,.uni-time-input,.uni-input-people,.uni-input-phone{
		font-family: Tensentype MingSongJ-W4;
		font-size: 24rpx;
		font-style: normal;
		font-weight: 400;
		line-height: 24rpx;
		letter-spacing: 0em;
		color: #000000;
		margin-right: 20rpx;
		
	}
	.list_textarea{
		margin: 0 20rpx;
		border-radius: 15rpx;
		border: 2rpx solid #F1F1F1;
		
		.textarea_content{
			padding: 5rpx 0 0 5rpx;
			color: #000000;
			font-family: Tensentype MingSongJ-W4;
			font-size: 24rpx;
			font-style: normal;
			font-weight: 400;
			line-height: 24rpx;
			letter-spacing: 0em;
			text-align: left;
		}
	}	
	.action-btn{
		position: fixed;
		bottom: 0;
		height: 100rpx;
		width: 100%;
		display: flex;	
		align-items: center;
		justify-content: center;
		background: #FF8D0E;
		color: #FFFFFF;
	}
</style>
