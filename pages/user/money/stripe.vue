<template>
	<view class="app">
		<web-view :src="url" ></web-view>
	</view>
</template> 
<script> 
import store from '@/store'
    export default {
        data() {
            return {
                url: 5, 
            };
        },
        computed: {},
        onLoad(options) {
			var token = store.state.estateToken || uni.getStorageSync('estateToken');
			this.url = '/hybrid/html/map.html?' +'&isLogin='+token
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
			console.log(this.url);
			currentWebview.append(w); 
        },
        methods: {
            // 数据初始化   
        }
    }
</script>

<style lang='scss'>
	  

</style>
