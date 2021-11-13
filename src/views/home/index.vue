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
  </div>
</template>

<script>
export default {
  name: 'index'
}
</script>

<style lang="less" scoped>
.home-container {
  .van-nav-bar__title {
    max-width: unset;
  }

  .search-btn {
    width: 555px;
    height: 64px;
    background-color: #5babfb;
  }
}
</style>
