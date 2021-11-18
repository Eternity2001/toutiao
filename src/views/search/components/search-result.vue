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
import { getSearchResults } from '@/api/search'

export default {
  name: 'search-result',
  props: {
    searchText: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      list: [],
      loading: false,
      finished: false,
      page: 1,
      perPage: 20,
      error: false
    }
  },
  methods: {
    async onLoad () {
      try {
        const { data } = await getSearchResults({
          page: this.page,
          per_page: this.per_page,
          q: this.searchText
        })

        // 将数据添加带数组中
        const { results } = data.data
        this.list.push(...results)
        console.log(this.list)

        this.loading = false

        // 判断还否还有数据
        if (results.length) {
          this.page++
        } else {
          this.finished = true
        }
      } catch (e) {
        this.$toast('数据获取失败')
      }
      // 请求获取数据
      // 关闭本次 loading
    }
  }
}
</script>

<style scoped>

</style>
