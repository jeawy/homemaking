<template>
	<view class="content">
		<!-- 地图 -->
       <web-view id="mapContainer" :src="url" @message="message"></web-view>
	</view>
</template>

<script>
	 
	// 发送给后端位置信息的接口
	
	export default {
		data() {
			return {
				baseurl: 'https://tl.chidict.com/',
				height: 0,
				width: 0,
				url:'https://video-7792-rzpczv.twil.io/index.html',
				id:'',
				uuid:'',
				images:''
			};
		},
		computed: {
			UserInfo() {
				return this.$store.state.UserInfo
			}
		},
		methods: {
			layerShow(){
				if(this.enable_satellite == false){
					uni.showModal({
					content: "是否打开卫星地图？",
					success: (e) => {
						if (e.confirm) {
							this.enable_satellite = true
						} else {
						}
					},
					fail: () => {
						res(false)
					}
				})
				}else{
					uni.showModal({
						content: "是否关闭卫星地图？",
						success: (e) => {
							if (e.confirm) {
								this.enable_satellite = false
							} else {
							}
						},
						fail: () => {
							res(false)
						}
					})
				}
				
			},
		  
		},
		onReady() {
		},
		onNavigationBarButtonTap(val) {
			if (val.index == 0) {
				// console.log("第一个按钮");
				this.shareIcon()
			};
		},
		onLoad(res) {
			search_users({
				userid: this.UserInfo.id
			}).then(({
				data
			}) => {
				this.images = this.baseurl + data.msg[0].thumbnail_portait
				this.id = res.id
				this.uuid = res.uuid
				uni.getSystemInfo({
					success: (res) => {
						this.width = res.windowWidth 
						this.height = res.windowHeight
					}
				})
				this.url = '/hybrid/html/detail.html?id='+this.id+'&img='+this.images+'&scale=11&from=singlemessage&isappinstalled=0'
				// this.url = this.baseurl + 'appshare/map.html?id='+this.id+'&scale=11&from=singlemessage&isappinstalled=0'
				let w = null;
				if (uni.getSystemInfoSync().platform =='ios'){
					// ios的全屏要特殊设置
					   w=plus.webview.create(this.url,'id',{
						  top:uni.getSystemInfoSync().statusBarHeight - 95,
						   height:uni.getSystemInfoSync().windowHeight + 65
					 },{preload:'preload webview'});
				}
				else{
					   w=plus.webview.create(this.url,'id',{   },{preload:'preload webview'});
				}
				let currentWebview = this.$mp.page.$getAppWebview(); 
				console.log(currentWebview);
				currentWebview.append(w);
			})
		},
	};
</script>
<style lang="scss" scoped>
	.cover-image {
		position: absolute;
		width: 50px;
		height: 50px;
		bottom:135rpx;
		left:15rpx;
	}
	.layer-image{
		position: absolute;
		width: 50px;
		height: 50px;
		bottom:35rpx; 
		left:15rpx;
	}
</style>
