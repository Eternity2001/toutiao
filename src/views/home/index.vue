<template>
  <div class="home-container">
    <!--导航栏-->
    <van-nav-bar class="page-nav-bar" fixed>
      <van-button
        slot="title"
        class="search-btn"
        icon="search"
        round
        size="small"
        type="info"
      >搜索
      </van-button>
    </van-nav-bar>
    <!-- 频道列表 -->
    <van-tabs v-model="active">
      <van-tab title="标签 1">内容 1</van-tab>
      <van-tab title="标签 2">内容 2</van-tab>
      <van-tab title="标签 3">内容 3</van-tab>
      <van-tab title="标签 4">内容 4</van-tab>
    </van-tabs>
    <!-- /频道列表 -->
  </div>
</template>

<script>
export default {
  name: 'index',
  data () {
    return {
      active: 2
    }
  }
}
</script>

<style lang="less" scoped>
.home-container {
  padding-top: 100px;
  padding-bottom: 100px;

  .van-nav-bar__title {
    max-width: unset;
  }

  .search-btn {
    width: 555px;
    height: 64px;
    background-color: #5babfb;
    border: none;
    font-size: 28px;

    .van-icon {
      font-size: 32px;
    }
  }
}
</style>
