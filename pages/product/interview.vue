<template>
	<view class="interview">
		<view class="top_line"></view>
		<!-- 立即预约 -->
		<form class="form" @submit="formSubmit">			
			<view class="list_name">
				预约的阿姨:{{infolist.name}}
			</view>
			<view class="list_people">
				<image class="uni-img" src="../../static/houseKeeping/mine.png"></image>
				<input class="uni-input-people" name="input" v-model="people" placeholder='请输入用户名称' />
			</view>
			<view class="list_phone">
				<image class="uni-img" src="../../static/houseKeeping/phone.png"></image>
				<input class="uni-input-phone" name="number" v-model="phone"
					maxlength='10' placeholder='请输入电话号码' />
			</view>
			<view class="list_dept">
				<image class="uni-img" src="../../static/houseKeeping/people.png"></image>
				<view v-if="deptarray.length == 0" class="line">
					<input class="uni-input" name="input" :disabled='true' placeholder="请选择工种" />
				</view>
				<view v-else  class="line">
					<picker mode="time" @change="bindPickerdept" :value="deptindex" :range="deptarray">
						<view class="uni-dept-input">{{deptarray[deptindex]}}</view>
					</picker>
				</view>				
			</view>
			<view class="list_time">
				<image class="uni-img" src="../../static/houseKeeping/time.png"> </image>
				<view v-if="deptarray.length == 0"  class="line">
					<input class="uni-input" name="input" :disabled='true' placeholder="请选择用工时间" />
				</view>
				<view v-else  class="line">
					<picker mode="time" :value="time" start="00:00" end="24:00" @change="bindTimeChange">
						<view class="uni-time-input">{{time}}</view>
					</picker>
				</view>
				
			</view>
			<view class="list_address">
				<image class="uni-img" src="../../static/houseKeeping/address.png"></image>
				<view v-if="deptarray.length == 0"  class="line">
					<input class="uni-input" name="input" :disabled='true' placeholder="请选择您的服务地址" />
				</view>
				<view v-else  class="line">
					<picker mode="time" @change="bindPickeraddress" :value="addreindex" :range="addrearray">
						<view class="uni-dept-input">{{addrearray[addreindex]}}</view>
					</picker>
				</view>
			</view>
			<view class="list_textarea">
				<textarea class="textarea_content" placeholder-style="color:#9e9e9e" placeholder="请输入您的服务内容"/>
			</view>
			<view class="action-btn">
				<view type="primary" class="action-btn no-border buy-now-btn">立即预约</view>
			</view>
		</form>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				infolist:{
					id:1,
					name:'张三',
					imgsrc:"/static/people.svg",
					age:27,
					sex:"women",
					position:'澳大利亚',
					time:"2",
					language:['普通话','英语'],
					type:"包月小时工"
				},
				phone:"15555555555",
				people:"ios端客户",
				time:"06:30",
				deptarray:['清洁工', '洗碗工'],
				deptindex:0,
				addrearray:['西安', '上海'],
				addreindex:0,
			}
		},
		onLoad() {
		},
		methods:{
			bindTimeChange: function(e) {
				this.time = e.target.value
			},
			bindPickerdept:function(e){
				 this.deptindex = e.target.value
			},
			bindPickeraddress:function(e){
				 this.addrearray = e.target.value
			},
			formSubmit: function(e){
				console.log('form发生了submit事件，携带数据为：' + JSON.stringify(e.detail.value))
				var formdata = e.detail.value
				uni.showModal({
					content: '表单数据内容：' + JSON.stringify(formdata),
					showCancel: false
				});
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
				
	}
	.uni-img{
		width: 60rpx;
		height: 60rpx;
	}
	.list_name{
		border-bottom: 2rpx solid #f1f1f1;
	}
	.list_name,.list_people,.list_phone,.list_dept,.list_time,.list_address{
		display:flex;
		align-items: center;
		height: 100rpx;
		padding: 0 20rpx;				
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
