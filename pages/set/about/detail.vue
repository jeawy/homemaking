<template>
	<view class="about">
		<view class="shop-info" v-if="title === '家政介绍'">
			<rf-image :src="detail.cover || detail.web_logo || ''"></rf-image>
			<view v-if="detail.title"><text>家政名称: </text><text>{{ detail.title }}</text></view>
			<view v-if="detail.address_name"><text>联系地址: </text><text>{{ detail.address_name }}</text></view>
			<view v-if="detail.address_details"><text>详细地址: </text><text>{{ detail.address_details }}</text></view>
			<view v-if="detail.mobile"><text>手机号码: </text><text>{{ detail.mobile }}</text></view>
			<view v-if="detail.qq"><text>QQ: </text><text>{{ detail.qq }}</text></view>
		</view>
		<view class="shop-info" v-if="title === '注册协议'">
			<view v-if="detail.protocol_register">
				<rich-text :nodes="detail.protocol_register | formatRichText"></rich-text>
			</view>
			<rf-empty :info="`暂无${title}`" v-if="!detail.protocol_register"></rf-empty>
		</view>
		<view class="shop-info" v-if="title === '隐私协议'">
			<view v-if="detail.protocol_privacy">
				<rich-text :nodes="detail.protocol_privacy | formatRichText"></rich-text>
			</view>
			<rf-empty :info="`暂无${title}`" v-if="!detail.protocol_privacy"></rf-empty>
		</view><web-view src="../../../hybrid/html/privacy.html"></web-view>
		<view class="shop-info" v-if="title === '充值协议'">
			<view v-if="detail.protocol_recharge">
				<rich-text :nodes="detail.protocol_recharge | formatRichText"></rich-text>
			</view>
			<rf-empty :info="`暂无${title}`" v-if="!detail.protocol_recharge"></rf-empty>
		</view>
		<rf-empty :info="`暂无${title}`" v-if="!detail && !loading"></rf-empty>
		<!--加载动画-->
		<rf-loading v-if="loading"></rf-loading>
	</view>
</template>

<script>
/**
* @des 关于家政详情
*
* @author stav stavyan@qq.com
* @date 2019-12-09 10:13
* @copyright 2019
*/
import {configList} from '@/api/basic';
import {merchantView} from '@/api/merchant';
export default {
	data() {
		return {
			detail: {},
			title: null,
			loading: true
		}
	},
	onLoad(options) {
		this.initData(options);
	},
	filters: {
	    /**
		 * 处理富文本里的图片宽度自适应
		 * 1.去掉img标签里的style、width、height属性
		 * 2.img标签添加style属性：max-width:100%;height:auto
		 * 3.修改所有style里的width属性为max-width:100%
		 * 4.去掉<br/>标签
		 * @param html
		 * @returns {void|string|*}
		 */
		formatRichText (html) { //控制小程序中图片大小
	    if (html) {
	        let newContent = html.replace(/<img[^>]*>/gi, function(match, capture) {
					if(match.search(/style=/gi) == -1){
						match = match.replace(/\<img/gi,'<img style=""');
					}
					return match;
				});
				newContent = newContent.replace(/style="/gi, '$& max-width:100% !important; ');
				newContent = newContent.replace(/<br[^>]*\/>/gi, '');
				return newContent;
	    } else {
	        return '暂无阿姨详情'
	    }
		},
	},
	methods: {
		// 数据初始化
		initData (options) {
			this.title = options.title;
			uni.setNavigationBarTitle({
				title: options.title
			});
			this.getConfigList(options.field);
		},
		// 获取家政详情
		async getConfigList(field) {
	    if (field.indexOf('protocol') !== -1) {
				await this.$http.get(`${configList}`, {
					field
				}).then(r => {
			    this.loading = false;
					this.detail = r.data
				}).catch(() => {
			    this.loading = false;
				});
	    } else {
			  const userInfo = uni.getStorageSync('userInfo');
		    if (!userInfo) return;
				await this.$http.get(`${merchantView}`, {
					id: userInfo.merchant_id,
					field
				}).then(r => {
			    this.loading = false;
					this.detail = r.data
				}).catch(() => {
			    this.loading = false;
				});
	    }
		}
	}
}
</script>
<style lang="scss">
	page{
		background-color: $page-color-base;
		position: relative;
		.about {
			padding-top: $spacing-lg;
			.shop-info {
				text-align: center;
				image {
					margin-top: 100upx;
					width: 240upx;
					height: 240upx;
					border-radius: 50%;
				}
			}
		}
	}
</style>
