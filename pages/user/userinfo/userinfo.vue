<template>
	<view class="userinfo">
		<!--头像 + 背景-->
		<view class="user-section">
			<image class="bg" :src="userBg"></image>
			<text class="bg-upload-btn yticon icon-paizhao"></text>
      <!--#ifdef H5-->
			<!--h5直接上传头像-->
			<view class="portrait-box" @tap="uploadImage">
				<image class="portrait" :src="baseurl+profileInfo.head_portrait || headImg"></image>
			</view>
      <!-- #endif -->
			<!--#ifndef H5-->
			<!--非h5裁剪头像上传-->
			<view class="portrait-box">
        <avatar
					canRotate="false"
          selWidth="200px" selHeight="400upx" @upload="handleUploadFile" :avatarSrc="baseurl+profileInfo.head_portrait || headImg"
          avatarStyle="width: 200upx; height: 200upx; border-radius: 100%; border: 6upx solid #fff;">
        </avatar>
			</view>
      <!-- #endif -->
		</view>
		<view class="input-content">
				<view class="input-item">
					<text class="tit">手机号</text>
					<input
						type="number"
						v-model="profileInfo.phone"
						disabled
						placeholder="请输入手机号码"
					/>
				</view>
				<view class="input-item">
					<text class="tit">昵　称</text>
					<input
						type="text"
						v-model="profileInfo.username"
						disabled
						placeholder="请输入您的昵称"
					/>
				</view>
				 
				<view class="input-item">
					<text class="tit">性　别</text>
					<radio-group @change="handlesexChange">
						<label class="sex-item" v-for="(item, index) in sexs" :key="index">
							<radio class="sex-item-radio" color="#fa436a" :value="item.value" :checked="item.value === profileInfo.sex" />
							<text class="sex-item-text">{{ item.name }}</text>
						</label>
					</radio-group>
				</view>
				<view class="input-item">
					<text class="tit">年　龄</text>
					<input
						v-model="profileInfo.age"
						placeholder="请输入您的年龄"
					/>
				</view>
				<view class="input-item">
					<text class="tit">生　日</text>
					<picker mode="date" v-model="date" @change="bindDateChange">
						<view class="date" style="background: none;">{{ date || '请选择日期' }}</view>
					</picker>
				</view>
				 
				<view class="input-item">
					<text class="tit">邮　箱</text>
					<input
						v-model="profileInfo.email"
						placeholder="请输入您的邮箱"
					/>
				</view>
				<view class="input-item">
					<text class="tit">工作年限</text>
					<input
						v-model="profileInfo.workyears"
						placeholder="请输入您的工作年限"
					/>
				</view>
				<view class="input-item">
					<text class="tit">语　言</text>
					<input
						v-model="profileInfo.languages"
						placeholder="请输入您的语言"
					/>
				</view>
				<button class="confirm-btn" :disabled="btnLoading" :loading="btnLoading" @tap="toUpdateInfo">修改资料</button>
		</view>

		<!--加载动画-->
		<rf-loading v-if="loading"></rf-loading>

    <!--进度条加载-->
		<rf-load-progress :height="CustomBar" :progress="loadProgress"></rf-load-progress>
	</view>
</template>

<script>
	/**
	 * @des 修改用户信息
	 *
	 * @author stav stavyan@qq.com
	 * @date 2020-01-10 14:28
	 * @copyright 2019
	 */
	import {memberInfo, memberUpdate, uploadImage} from '@/api/userInfo';
	import avatar from '@/components/rf-avatar/rf-avatar';
	import moment from '@/common/moment';
	import dayjs from "dayjs";
	export default {
		components: { avatar },
		data() {
			return {
				loadProgress: 0,
				CustomBar: this.CustomBar,
				profileInfo: {},
				sexs: [
					{
						value: '0',
						name: '未知'
					},
					{
						value: '男',
						name: '男'
					},
					{
						value: '女',
						name: '女'
					}],
				date: moment().format('YYYY-MM-DD'),
				token: null,
				loading: true,
				headImg: this.$mAssetsPath.headImg,
				userBg: this.$mAssetsPath.userBg,
				btnLoading: false,
				baseurl: "",
				userInfo : ""
			};
		},
		onLoad () {
			this.initData()
			this.baseurl = this.$mStore.state.BaseUrl
		},
		methods: {
			dateTimeFormat(date){
				if (date) {
						date *= 1000
						return dayjs(date).format("YYYY-MM-DD")
					} else {
						return ''
				}
			},
			// 上传头像
			uploadImage () {
				// 从相册选择图片
				const _this = this;
				uni.chooseImage({
					count: 1,
					sizeType: ['original', 'compressed'],
					sourceType: ['album'],
					success: function(res) {
						_this.handleUploadFile(res.tempFilePaths);
					}
				});
			},
			// 上传头像
			handleUploadFile (data) {
				const _this = this;
		    let filePath = data.path || data[0];
			_this.$http.upload(uploadImage, {filePath, name: 'file'}).then(r => {
								_this.profileInfo.head_portrait = r.msg;
								_this.userInfo = this.$mStore.state.userInfo
								_this.userInfo.head_portrait = r.msg
								console.log(_this.userInfo)
								uni.setStorageSync('userInfo', _this.userInfo);
								//_this.handleUpdateInfo(_this.profileInfo);
			});
			},
			// 监听日期更改
			bindDateChange(e) {
				this.date = e.target.value
			},
			// 监听性别更改
			handlesexChange (e) {
		    this.profileInfo.sex = e.detail.value;
			},
			// 数据初始化
			initData(){
		    this.token = uni.getStorageSync('accessToken') || undefined;
				this.getMemberInfo()
			},
			// 获取用户信息
			async getMemberInfo () {
				await this.$http.get(memberInfo).then(r => {
					console.log(r)
			    this.loading = false;
					this.profileInfo = r.msg;
					this.profileInfo.languages = this.profileInfo.languages.join(' ')
					this.date = this.dateTimeFormat(this.profileInfo.birthday);
				}).catch(() => {
			    this.loading = false;
				})
			},
			// 更新用户信息
			async toUpdateInfo(){
				this.handleUpdateInfo();
			},
			// 更新用户信息
			async handleUpdateInfo () {
				if(!(/(^[1-9]\d*$)/.test(this.profileInfo.age))){
					this.$mHelper.toast('年龄必须为正整数');
					return;
				}
				if(!(/(^[1-9]\d*$)/.test(this.profileInfo.workyears))){
					this.$mHelper.toast('工作年限必须为正整数');
					return;
				}
				this.btnLoading = true;
				this.loadProgress = this.loadProgress + 6;
				const timer = setInterval(() => {
						this.loadProgress = this.loadProgress + 6;
				}, 50);
				await this.$http.post(`${memberUpdate}?userid=${this.profileInfo.userid}`, {
					...this.profileInfo,
					birthday: this.date,
					method : "put"
				}).then((r) =>{
					console.log(r)
					clearInterval(timer);
					this.loadProgress = 0;
					if (r.status == 1){
						this.$mHelper.toast(r.msg); 
					}else{
						this.$mHelper.toast(r.msg);
						setTimeout(() => {
							this.$mRouter.back();
						}, 1 * 3000);
					} 
				    this.btnLoading = false;
				}).catch((r) => {
					console.log(r)
					this.btnLoading = false;
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	page {
		background-color: $color-white;
	}
	.userinfo{
		.user-section{
			display:flex;
			align-items:center;
			justify-content: center;
			height: 300upx;
			padding: 40upx 30upx 0;
			overflow: hidden;
			position:relative;
			.bg{
				position:absolute;
				left: 0;
				top: 0;
				width: 100%;
				height: 100%;
				filter: blur(1px);
				opacity: .7;
			}
			.portrait-box{
				clear: both;
				z-index: 2;
			}
			.portrait{
				position: relative;
				width: 200upx;
				height: 200upx;
				border-radius: 50%;
	      border: 6upx solid #fff;
			}
			.yticon{
				position:absolute;
				line-height: 1;
				z-index: 5;
				font-size: 48upx;
				color: #fff;
				padding: 4upx 6upx;
				border-radius: 6upx;
				background: rgba(0,0,0,.4);
			}
			.pt-upload-btn{
				right: 0;
				bottom: 10upx;
			}
			.bg-upload-btn{
				right: 20upx;
				bottom: 16upx;
			}
		}
		.input-content{
			padding: 40upx 60upx;
			.input-item{
				display:flex;
				padding: 0 30upx;
				background: $page-color-light;
				height: 80upx;
				line-height: 80upx;
				border-radius: 4px;
				margin-bottom: 30upx;
				&:last-child{
					margin-bottom: 0;
				}
				.tit{
					width: 110upx;
					font-size: $font-sm+2upx;
					color: $font-color-base;
				}
				input {
					height: 80upx;
					line-height: 80upx;
					font-size: $font-base + 2upx;
					color: $font-color-dark;
					width: 300upx;
				}
				.date {
					height: 80upx;
					line-height: 80upx;
					font-size: $font-base + 2upx;
					color: $font-color-dark;
				}
				.sex-item {
					margin-right: 20upx;
					.sex-item-text {
						padding: 0 5upx;
					}
					radio .wx-radio-input.wx-radio-input-checked {
						background: $uni-color-primary;
						border-color: $uni-color-primary;
					}
				}
			}
		}
	}
</style>
