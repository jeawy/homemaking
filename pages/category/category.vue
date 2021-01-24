<template>
  <view class="rf-category">
    <!--顶部搜索导航栏-->
    <view class="nev">
      <view class="search">
        <view class="search_position" @tap="positionSearch">
          <view class="left_text">西安</view>
          <image class="location_img" src="/static/home/City.svg"></image>
        </view>
        <view class="search_box" @tap="toSearch">
          <image
            class="search_img"
            src="/static/houseKeeping/search.svg"
          ></image>
          <input class="search_input" placeholder="搜索关键字" />
          <image
            class="search_talk"
            src="/static/houseKeeping/microphone.svg"
          ></image>
        </view>
        <view class="notice" @tap="navTo('/pages/user/notice/notice')">
          <image class="notice_img" src="/static/home/Notification.svg"></image>
        </view>
      </view>
    </view>
    <!-- 上侧分类导航 -->
    <view class="top_search">
      <view class="top">
        <view class="top_left">
          <view
            class="left_one"
            @tap="tabClick('recommend')"
            :class="{ tab_active: currentIndex === 'recommend' }"
            >推荐</view
          >
          <view
            class="left_two"
            @tap="tabClick('new')"
            :class="{ tab_active: currentIndex === 'new' }"
            >最新</view
          >
          <view
            class="left_three"
            @tap="tabClick('nearby')"
            :class="{ tab_active: currentIndex === 'nearby' }"
            >附近</view
          >
        </view>
        <view class="top_right">
          <view
            class="right_one"
            @tap="tabRightClick('screen')"
            :class="{ tab_active: tabIndex === 'screen' }"
            >筛选</view
          >
          <image
            class="right_img"
            src="/static/houseKeeping/triangle.svg"
          ></image>
          <view
            class="right_two"
            @tap="tabRightClick('classify')"
            :class="{ tab_active: tabIndex === 'classify' }"
            >分类</view
          >
          <image
            class="right_img"
            src="/static/houseKeeping/triangle.svg"
          ></image>
        </view>
      </view>
    </view>

    <view class="main_content">
      <!-- 推荐页面 -->
      <view v-if="currentIndex == 'recommend'">
        <rf-empty
          :info="'暂无阿姨数据~'"
          v-if="infolst_recommend.length === 0"
        ></rf-empty>
        <scroll-view scroll-y="true" v-else>
          <view class="content">
            <mainCard
              class="main_card"
              :info="item"
              v-for="(item, index) in infolst_recommend"
              :key="index"
            />
          </view>
        </scroll-view>
      </view>
      <!-- 最新页面-->
      <view v-if="currentIndex == 'new'">
        <rf-empty
          :info="'暂无阿姨数据~'"
          v-if="infolst_new.length === 0"
        ></rf-empty>
        <scroll-view scroll-y="true" v-else>
          <view class="content">
            <mainCard
              class="main_card"
              :info="item"
              v-for="(item, index) in infolst_new"
              :key="index"
            />
          </view>
        </scroll-view>
      </view>
      <!-- 附近页面-->
      <view v-if="currentIndex == 'nearby'">
        <rf-empty
          :info="'暂无阿姨数据~'"
          v-if="infolst_nearby.length === 0"
        ></rf-empty>
        <scroll-view scroll-y="true" v-else>
          <view class="nearby_map">
            <map />
          </view>
          <view class="content">
            <mainCard
              class="main_card"
              :info="item"
              v-for="(item, index) in infolst_nearby"
              :key="index"
            />
          </view>
        </scroll-view>
      </view>
    </view>
    <s-popup custom-class="demo-popup" position="center" v-model="visible">
      <!-- 内容 -->
      <!-- 筛选弹框 -->
      <view class="django_search">
        <view class="django_header">筛选</view>
        <form @submit="confirm" @reset="clear">
          <view class="experience">
            <view class="django_title">经验要求</view>
            <view class="django_button">
              <view
                class="django_text"
                @tap="selectOne(index, item,'search')"
                :class="{ django_active: item.checked == true }"
                v-for="(item, index) in experienceLst"
                :key="index"
                >{{ item.label }}
              </view>
            </view>
           </view>
          <view class="language">
            <view class="django_title">语言要求</view>
            <view class="django_button">
              <view
                class="django_text"
                @tap="selectTwo(index, item,'search')"
                :class="{ django_active: item.checked == true }"
                v-for="(item, index) in languageLst"
                :key="index"
                >{{ item.value }}
              </view>
            </view>
          </view>
          <view class="price">
            <view class="django_title">
              <text>价格筛选</text>
              <text>(单选)</text>
            </view>
            <view class="django_button">
              <view
                class="django_text"
                @tap="selectThree(index, item)"
                :class="{ django_active: selectthree === index }"
                v-for="(item, index) in priceLst"
                :key="index"
                >{{ item.label }}
              </view>
            </view>
          </view>
          <view class="tool">
            <view class="django_title">
              <text>工具</text>
              <text>(单选)</text>
            </view>
            <view class="django_button">
              <view
                class="django_text"
                @tap="selectFour(index, item)"
                :class="{ django_active: selectfour === index }"
                v-for="(item, index) in toolLst"
                :key="index"
                >{{ item.label }}
              </view>
            </view>
          </view>
          <view class="menu">
            <view
              class="clear_button"
              :class="{ click_button: clearButton == 'clear' }"
              @tap="clear"
            >
              <text class="menu_text">清除</text>
            </view>
            <view
              class="confirm_button"
              :class="{ click_button: confirmButton == 'confirm' }"
              @tap="confirm"
            >
              <text class="menu_text">确认</text>
            </view>
          </view>
        </form>
      </view>
    </s-popup>
	<!--分类-->
	<s-popup custom-class="demo-popup" position="center" v-model="classVisiable">
		<categoryLists />
	</s-popup>
    <!-- 城市定位 -->
    <s-popup custom-class="demo-popup" position="center" v-model="cityVisiable">
      <citySearch />
    </s-popup>
  </view>
</template>
<script>
import mainCard from "@/components/main-card.vue";
import sPopup from "@/components/s-popup/index.vue";
import citySearch from "@/components/city-search.vue";
import categoryLists from '@/components/category-lists.vue';
/**
 * @des 分类
 *
 * @author stav stavyan@qq.com
 * @date 2020-03-10 18:19
 * @copyright 2019
 */
import { productCate, categoryList } from "@/api/product";
import rfSearchBar from "@/components/rf-search-bar";
import { advList } from "@/api/basic";
export default {
  components: {
    rfSearchBar,
    mainCard,
    sPopup,
    citySearch,
	categoryLists
  },
  data() {
    return {
      // hotSearchDefault: '请输入关键字', // 搜索默认关键字
      // showCategoryIndex: 0, // 一级菜单高亮显示序号
      // search: {},
      // cateTop: {}, // 分类通用广告图
      // animation: 'animation-slide-right',
      // errorImage: this.$mAssetsPath.errorImage,
      currentIndex: "recommend",
      tabIndex: "",
      visible: false,
      cityVisiable: false,
	  classVisiable: false,
      infolst_recommend: [
        {
          id: 1,
          name: "张三",
          imgsrc: "/static/people.svg",
          age: 27,
          sex: "women",
          position: "澳大利亚",
          time: "2",
          language: ["普通话", "英语"],
          type: "包月小时工",
        },
        {
          id: 2,
          name: "爱丽丝",
          imgsrc: "/static/people.svg",
          age: 27,
          sex: "women",
          position: "澳大利亚",
          time: "2",
          language: ["普通话", "英语"],
          type: "包月小时工",
        } 
      ],
      infolst_new: [
        {
          id: 1,
          name: "marry",
          imgsrc: "/static/people.svg",
          age: 27,
          sex: "women",
          position: "澳大利亚",
          time: "2",
          language: ["普通话", "英语"],
          type: "包月小时工",
        } 
      ],
      infolst_nearby: [],
      experienceLst: [
        {
          value: "不限",
          label: "不限",
          checked: true,
        },
        {
          label: "1-3年",
          value: "1-3",
          checked: false,
        },
        {
          label: "3-5年",
          value: "3-5",
          checked: false,
        },
        {
          label: "5-10年",
          value: "5-10",
          checked: false,
        },
        {
          label: "10年以上",
          value: "10",
          checked: false,
        },
      ],
      languageLst: [
        {
          value: "不限",
          checked: true,
        },
        {
          value: "英文",
          checked: false,
        },
        {
          value: "普通话",
          checked: false,
        },
        {
          value: "广东话",
          checked: false,
        },
        {
          value: "闽南话",
          checked: false,
        },
      ],
      priceLst: [
        { label: "不限", value: "不限" },
        { label: "20/h", value: "20" },
        { label: "40/h", value: "40" },
        { label: "60/h", value: "60" },
        { label: "80/h", value: "80" },
      ],
      toolLst: [
        { label: "不限", value: "不限" },
        { label: "自带", value: "0" },
        { label: "雇主提供", value: "1" },
      ],
      selectone: 0,
      selecttwo: 0,
      selectthree: 0,
      selectfour: 0,
      clearButton: "",
      confirmButton: "",
      searchParams: {
        workyears:'',
        language:'',
      },
    };
  },
  onShow(options) {  
        this.initData( );  
		}, 
  methods: {
    // 通用跳转
    navTo(route) {
      this.$mRouter.push({ route });
    },
    // 定位搜索
    positionSearch() {
      this.cityVisiable = !this.cityVisiable;
    },
    // 顶部tab点击
    tabClick(index) {
      this.currentIndex = index;
    },
    tabRightClick(index) {
      this.tabIndex = index;
      if (index == "screen") {
        this.visible = !this.visible;
      }
	  if (index == "classify"){
		  this.classVisiable = !this.classVisiable;
	  }
    },
    // 点击筛选框中的取消按钮
    clear() {
      this.clearButton = "clear";
      this.confirmButton = "";
      this.searchParams = {}
      this.experienceLst.forEach((item,index) =>{
        if(index == 0){
          this.selectOne(index,item,"clear")
        }        
      })
      this.languageLst.forEach((item,index) =>{
        if(index == 0){
          this.selectTwo(index,item,"clear")
        }        
      })
      this.selectthree = 0
      this.selectfour = 0
    },
    // 点击筛选框中的确定按钮进行搜索
    confirm() {
      this.confirmButton = "confirm";
      this.clearButton = "";
      this.searchParams.workyears = ''
      this.searchParams.language = ''
      for(var i in this.searchParams){
        if(this.searchParams[i] == '不限'){
          this.$delete(this.searchParams,i)
        }        
      }
      this.experienceLst.forEach(element => {
        if(element.checked == true){
          this.searchParams.workyears += element.value+','
        }
      });
      this.searchParams.workyears = this.searchParams.workyears.substring(0,this.searchParams.workyears.lastIndexOf(','))
      this.languageLst.forEach(element => {
        if(element.checked == true){
          this.searchParams.language += element.value+','
        }
      });
      this.searchParams.language = this.searchParams.language.substring(0,this.searchParams.language.lastIndexOf(','))
      this.searchCate(this.searchParams);
    },
    // 搜索阿姨列表
    searchCate(data) {
      this.$http.get(`${categoryList}`, data).then(({status,msg}) => {
        if(status===0){
          this.visible = false
        }
       
          this.infolst_recommend = msg;
          this.infolst_new = msg;
		  this.languagesToString(this.infolst_recommend);
		  this.languagesToString(this.infolst_new);
        })
        .catch(() => {});
    },
    // 选择经验要求
    selectOne(index, item,type) {
      if(index == 0){
        if(type == 'search'){
          item.checked = !item.checked
        }else{
          item.checked = true
        }        
        for(var i=1;i<this.experienceLst.length;i++){
          this.experienceLst[i].checked = false
        }        
      }else{
        this.experienceLst[0].checked = false
        item.checked = !item.checked
      }
    },
    // 选择语言要求
    selectTwo(index, item,type) {
      if(index == 0){
        if(type == 'search'){
          item.checked = !item.checked
        }else{
          item.checked = true
        }   
        // item.checked = !item.checked
        for(var i=1;i<this.languageLst.length;i++){
          this.languageLst[i].checked = false
        }  
      }else{
        this.languageLst[0].checked = false
        item.checked = !item.checked
      }
    },
    // 选择价格要求
    selectThree(index, item) {
      this.selectthree = index;
      this.searchParams.price = item.value;
    },
    // 选择工具要求
    selectFour(index, item) {
      this.selectfour = index;
      this.searchParams.tools = item.value;
    },
    // 数据初始化
    async initData(categoryid = 0) {
      this.search = uni.getStorageSync("search") || {};
      this.hotSearchDefault =
        "请输入关键字 " +
        (this.search.hot_search_default
          ? "如: " + this.search.hot_search_default
          : ""); 
      this.getProductCate();
      //   this.initCartItemCount();
    },
    // 设置购物车数量角标
    async initCartItemCount() {
      if (
        this.$mStore.getters.hasLogin &&
        parseInt(uni.getStorageSync("cartNum"), 10) > 0
      ) {
        await uni.setTabBarBadge({
          index: this.$mConstDataConfig.cartIndex,
          text: uni.getStorageSync("cartNum").toString(),
        });
      } else {
        uni.removeStorageSync("cartNum");
        uni.removeTabBarBadge({ index: this.$mConstDataConfig.cartIndex });
      }
    },
    // 获取商品分类列表
    async getProductCate() {
      let categoryid_param = 0;
      let categoryid =  uni.getStorageSync("categoryid") 
      if (categoryid) {
        if (categoryid > 0 ){
           categoryid_param = categoryid
        }
      } 
      await this.$http
            .get(`${categoryList}` ,{categoryid:categoryid_param} )
            .then((r) => {
              this.infolst_recommend = r.msg;
              this.infolst_new = r.msg;
                this.languagesToString(this.infolst_recommend);
                this.languagesToString(this.infolst_new);
            })
            .catch(() => {});
    },
	
	//将语言数组转换为字符串形式
	languagesToString(info){
		for(var i=0;i<info.length;i++){
			info[i].languages = info[i].languages.join(' ')
		}
	},
	
    
    //分类切换显示
    showCategory(index) {
      this.showCategoryIndex = index;
      this.animation = "animation-slide-right";
      setTimeout(() => {
        this.animation = "";
      }, 1000);
    },
    // 跳转至搜索详情页
    toSearch() {
      this.$mRouter.push({
        route: `/pages/index/search/search?data=${JSON.stringify(this.search)}`,
      });
    } 
  },
};
</script>
<style scoped lang="scss">
page {
  background-color: #ffffff;
}
.rf-category {
  background-color: #ffffff;
  // .top{}
  .search,
  .search_position,
  .search_box,
  .top,
  .top_left,
  .top_right {
    display: flex;
    align-items: center;
  }
  .nev {
    top: 70rpx; //设置距离顶部的距离
    height: 48rpx;
    position: fixed;
    width: 100%;
    z-index: 100;
    background-color: #ffffff;
  }
  .top_search {
    padding-top: 114rpx; //设置距离顶部的距离
    position: fixed;
    width: 100%;
    z-index: 90;
    background-color: #ffffff;
  }
  .main_content {
    position: relative;
    padding-top: 164rpx; //设置距离顶部的距离
    width: 100%;
    background-color: #ffffff;
  }
  .left_text,
  .search_input {
    font-family: Tensentype MingSongJ-W2;
    font-size: 24rpx;
    font-style: normal;
    font-weight: 400;
    line-height: 28rpx;
    letter-spacing: 0em;
  }
  .search {
    margin: 0 40rpx 10rpx 40rpx;
    // height: 40rpx;

    .search_position {
      .location_img {
        margin-left: 4rpx;
        width: 16rpx;
        height: 24rpx;
      }
    }
    .search_box {
      margin-left: 30rpx;
      width: 520rpx;
      background: rgba(118, 118, 128, 0.12);
      border-radius: 10px;
      .search_img {
        width: 22rpx;
        height: 22rpx;
        margin: 10rpx 12.2rpx 8rpx 12rpx;
      }
      .search_input {
        width: 446rpx;
      }
      .search_talk {
        width: 14.78rpx;
        height: 22rpx;
        margin: 10rpx 12.2rpx 8rpx 0;
      }
    }
    .notice {
      .notice_img {
        margin-left: 30rpx;
        width: 22rpx;
        height: 21.54rpx;
      }
    }
  }

  .left_one,
  .left_two,
  .left_three,
  .right_one,
  .right_two {
    font-family: Tensentype MingSongJ-W4;
    font-size: 20rpx;
    font-style: normal;
    font-weight: 400;
    line-height: 24rpx;
    letter-spacing: 0em;
    text-align: left;
  }
  .top {
    height: 32rpx;
    margin: 10rpx 40rpx 8rpx 40rpx;
    padding-bottom: 8rpx;
    border-bottom: 2rpx solid #ff8d0e;

    .top_left {
      .left_two {
        padding: 0 20rpx;
      }
    }
    .tab_active {
      color: #ff8d0e;
    }
    .top_right {
      padding-left: 320rpx;
      .right_img {
        margin: 6rpx 20rpx 0 4rpx;
        width: 10rpx;
        height: 10rpx;
      }
    }
  }
  .nearby_map {
    margin: 10rpx 20rpx;
    map {
      width: 100%;
      height: 460rpx;
    }
  }
  .content {
    display: flex;
    width: 100%;
    margin-left: 20rpx;
    flex-wrap: wrap;
    .main_card {
      margin-right: 22rpx;
      margin-bottom: 20rpx;
    }
  }
  .demo-popup {
    .s-popup-wrap {
      height: 546rpx;
    }
  }
  // 弹框样式
  .django_search {
    width: 620rpx;
    .django_header {
      align-items: center;
      font-family: Tensentype MingSongJ-W8;
      font-size: 12px;
      font-style: normal;
      font-weight: 900;
      line-height: 14px;
      letter-spacing: 0em;
      text-align: center;
      text-align: center;
      margin-top: 20rpx;
    }
    .django_title,
    .django_text,
    .django_active {
      font-family: Tensentype MingSongJ-W4;
      font-style: normal;
      font-weight: 400;
      letter-spacing: 0em;
    }
    .django_title {
      margin-bottom: 10rpx;
      margin-left: 20rpx;

      line-height: 28rpx;
      & > text:nth-child(1) {
        font-size: 24rpx;
      }
      & > text:nth-child(2) {
        font-size: 20rpx;
        margin-left: 14rpx;
      }
    }
    .django_text {
      font-size: 9px;
      line-height: 11px;
      margin: 0 10rpx;
      width: 100rpx;
      height: 32rpx;
      align-items: center;
      color: #ffffff;
      background: #767680;
      border-radius: 4rpx;
      box-sizing: border-box;
      display: flex;
      justify-content: center;
    }
    .django_active {
      color: #fdb667;
      border: 2rpx solid #fdb667;
      background: #ffffff;
    }
    .experience,
    .language,
    .price,
    .tool {
      height: 70rpx;
    }
    .experience {
      margin: 26rpx 0 30rpx 0;
    }
    .language,
    .price {
      margin: 30rpx 0 0 0;
    }
    .price {
      // margin: 30rpx 20rpx 0 20rpx;
    }
    .tool {
      margin: 34rpx 0 22rpx 0;
    }
    .django_button,
    .menu,
    .clear_button,
    .confirm_button {
      display: flex;
      align-items: center;
    }
    .django_button {
      margin-left: 10rpx;
    }
    .menu {
      justify-content: space-between;
      height: 80rpx;
      box-shadow: inset 0 2px 2px rgba(0, 0, 0, 0.25);
      background: #ffffff;
      .clear_button,
      .confirm_button {
        width: 140rpx;
        height: 60rpx;
        background: #767680;
        border-radius: 4rpx;
        color: #ffffff;
        justify-content: center;
      }
      .click_button {
        background: #ffa23a;
      }
      .clear_button {
        margin: 10rpx 0 10rpx 18rpx;
      }
      .confirm_button {
        margin: 10rpx 18rpx 10rpx 0;
      }
    }
    .menu_text {
      font-family: Tensentype MingSongJ-W4;
      font-size: 28rpx;
      font-style: normal;
      font-weight: 400;
      line-height: 34rpx;
      letter-spacing: 0em;
      text-align: center;
    }
  }
  .category-list {
	  width: 680rpx;
	  margin: 0;
  }
}
</style>
