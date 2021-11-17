<template>
  <div>
    <van-list
      v-model="loading"
      :error.sync="error"
      :finished="finished"
      error-text="加载失败，请点击重试"
      finished-text="没有更多了"
      @load="onLoad"
    >
      <van-cell
        v-for="(article, index) in list"
        :key="index"
        :title="article.title"
      />
    </van-list>
  </div>
</template>

<script>
export default {
  name: 'search-result',
  data () {
    return {
      list: [1, 2, 3, 4, 5, 6, 7, 8, 9],
      loading: false,
      finished: false,
      page: 1,
      perPage: 20,
      error: false
    }
  },
  methods: {
    onLoad (val) {
      console.log(val, 'duangduangduangduangduangduangduangduangduangduang')
    }
  }
}
</script>

<style scoped>

</style>
