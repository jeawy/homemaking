<template>
  <view class="collection">
    <view class="list-wrap">
      <mainCard
        v-for="(item, index) in collectionList"
        :info="item.product"
        :key="index"
        class="main_card"
      />
    </view>
    <rf-empty
      :info="'快去收藏吧~'"
      v-if="collectionList.length === 0 && !loading"
    ></rf-empty>
    <!--加载动画-->
    <rf-loading v-if="loading"></rf-loading>
  </view>
</template>

<script>
import { productLikes } from "@/api/likes";
import mainCard from "@/components/main-card.vue";
export default {
  components: {
    mainCard,
  },
  data() {
    return {
      collectionList: [],
      loading: false,
    };
  },
  onShow() {
    this.getCollectionList();
  },
  //下拉刷新
  onPullDownRefresh() {
    this.getCollectionList();
  },
  methods: {
    // 获取足迹列表
    getCollectionList() {
      this.loading = true;
      this.$http
        .get(`${productLikes}`, {
          likestype: 0, // 足迹传1，收藏传0
          entity: 1, // 阿姨固定传1
        })
        .then(({ status, msg }) => {
          if (status !== 0) {
            return;
          }
          this.collectionList = msg;
        })
        .finally(() => {
          this.loading = false;
        });
    },
  },
};
</script>

<style lang="scss">
page {
  background-color: $page-color-base;
}
.collection {
  .list-wrap {
    display: flex;
    width: 750rpx;
    padding-left: 20rpx;
    flex-wrap: wrap;
    .main_card {
      margin-right: 20rpx;
      margin-bottom: 20rpx;
    }
  }
}
</style>
