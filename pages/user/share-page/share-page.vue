<template>
	<view>
		<!-- 分享网页链接 -->
		<view>
			<web-view :src="'https://track.chidict.com/invite2.html?invite='"></web-view>
		</view>
		<!-- 分享邀请码图片（包含二维码） -->
		<!-- <view class="post">
			<view class="wrapper">
				<canvas style="width: 100%;height:100%;" canvas-id="firstCanvas"></canvas>
			</view>
		</view> -->

	</view>
</template>

<script>
	import share from "@/common/share.js"
	import {
		ApiPostInviteCode
	} from '@/api/currency.js'

	export default {

		data() {
			return {
				inviteCode: '',
				// imageBgURL: '../../../static/inviteQRbg.png',
				// qr_path: 'https://track.chidict.com/static/img/erweima.png',
				width: 0,
				height: 0,
				// provider: '',
				// fileImagePath: '',
			}
		},

		onBackPress() {
			//监听back键，关闭弹出菜单
			if (this.shareObj.shareMenu.isVisible()) {
				this.shareObj.shareMenu.hide();
				this.shareObj.alphaBg.hide();
				this.back();
				return true
			} else {
				this.back();
				return true
			}
		},
		onReady() {
			var _this = this;
			let system_info = uni.getSystemInfoSync();
			_this.width = system_info.screenWidth
			_this.height = system_info.screenHeight
			// var context = uni.createCanvasContext('firstCanvas')
			// context.drawImage('../../../static/inviteQRbg.png', 0, 0, system_info.screenWidth, system_info.screenWidth * 1000 / 750)
			// context.drawImage(_this.qr_path, system_info.screenWidth * 0.35, system_info.screenWidth * 850 / 750, 120, 120);
			// context.draw()
		},
		onLoad() {
			this.getInviteCode()
			setTimeout(() => {
				this.shareInfo()
			}, 4000)
		},
		onNavigationBarButtonTap(val) {
			if (val.index == 0) {
				// console.log("第一个按钮");
				this.shareInfo()
			};
		},
		methods: {
			// 获取邀请码
			getInviteCode() {
				ApiPostInviteCode({
					method: 'post',
				}).then(({
					data
				}) => {
					this.inviteCode = data.code
				})
			},
			back() {
				uni.switchTab({
					url: '/pages/user-center/my-account'
				});
			},

			shareInfo() {
				// 分享网页链接
				let shareInfo = {
					href: `https://track.chidict.com/invite.html?invite=${this.inviteCode}`,
					title: "车迹APP注册邀请",
					desc: "Hi,我在用车迹记录我的行车轨迹,注册送积分,积分兑好礼,你也来试试吧.",
					imgUrl: "https://track.chidict.com/static/img/erweima.png"
				};
				let shareList = [{
						icon: "/static/sharemenu/wx.png",
						text: "微信好友",
					},
					{
						icon: "/static/sharemenu/pyq.png",
						text: "朋友圈"
					},
					/*
					{
						icon: "/static/sharemenu/weibo.png",
						text: "微博"
					},
					{
						icon: "/static/sharemenu/qq.png",
						text: "QQ"
					},
					
					{
						icon: "/static/sharemenu/copy.png",
						text: "复制"
					},*/
					{
						icon: "/static/sharemenu/more.png",
						text: "更多"
					},
				];
				this.shareObj = share(shareInfo, shareList, function(index) {
					console.log("点击按钮的序号: " + index);
					let shareObj = {
						href: shareInfo.href || "",
						title: shareInfo.title || "",
						summary: shareInfo.desc || "",
						success: (res) => {
							console.log("success:" + JSON.stringify(res));
						},
						fail: (err) => {
							console.log("fail:" + JSON.stringify(err));
						}
					};
					switch (index) {
						case 0:
							shareObj.provider = "weixin";
							shareObj.scene = "WXSceneSession";
							shareObj.type = 0;
							shareObj.imageUrl = shareInfo.imgUrl || "";
							uni.share(shareObj);
							break;
						case 1:
							shareObj.provider = "weixin";
							shareObj.scene = "WXSenceTimeline";
							shareObj.type = 0;
							shareObj.imageUrl = shareInfo.imgUrl || "";
							uni.share(shareObj);
							break;
							/*
							case 2:
								shareObj.provider = "sinaweibo";
								shareObj.type = 0;
								shareObj.imageUrl = shareInfo.imgUrl || "";
								uni.share(shareObj);
								break;
							case 3:
								shareObj.provider = "qq";
								shareObj.type = 1;
								shareObj.imageUrl = shareInfo.imgUrl || "";
								uni.share(shareObj);
								break;
								*/
						case 2:
							uni.setClipboardData({
								data: shareInfo.href,
								complete() {
									uni.showToast({
										title: "已复制到剪贴板"
									})
								}
							})
							break;
						case 3:
							plus.share.sendWithSystem({
								type: "web",
								title: shareInfo.title || "",
								thumbs: [shareInfo.imgUrl || ""],
								href: shareInfo.href || "",
								content: shareInfo.desc || "",
							})
							break;
					};
				});
				this.$nextTick(() => {
					this.shareObj.alphaBg.show();
					this.shareObj.shareMenu.show();
				})

			},
		}
	}
</script>

<style lang="scss">
	.content {
		width: 750rpx;
		height: 180rpx;
		position: fixed;
		bottom: 0rpx;
		text-align: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}

	.addresslist {
		background-color: #DF5000;
		border-radius: 50upx;
		margin-top: 7%;
		/* position:relative; */
		width: 300rpx;
	}

	.post {
		height: 100%;

		// background-color: #f4f4f4;
		.wrapper {
			width: 100%;
			height: 1300rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			background-color: #FFFFFF;
		}
	}
</style>
