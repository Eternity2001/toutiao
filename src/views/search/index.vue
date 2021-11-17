<template>
  <div class="search-container">
    <!-- 搜索框 -->
    <form action="/" class="search-form">
      <van-search
        v-model="searchText"
        background="#3296fa"
        placeholder="请输入搜索关键词"
        show-action
        @cancel="onCancel"
        @focus="isResultShow = false"
        @search="onSearch"
      />
    </form>
    <!-- /搜索框 -->

    <!-- 搜索结果 -->
    <search-result v-if="isResultShow"/>
    <!-- /搜索结果 -->

    <!-- 联想建议 -->
    <search-suggestion v-else-if="searchText"/>
    <!-- /联想建议 -->

    <!-- 历史记录 -->
    <search-history v-else/>
    <!-- /历史记录 -->
  </div>
</template>

<script>
import SearchHistory from './components/search-history'
import SearchResult from './components/search-result'
import SearchSuggestion from './components/search-suggestion'

export default {
  name: 'index',
  components: {
    SearchHistory,
    SearchResult,
    SearchSuggestion
  },
  data () {
    return {
      searchText: '',
      isResultShow: false // 控制搜索结果的展示
    }
  },
  methods: {
    onSearch (val) {
      console.log(val)
    },
    onCancel () {
      this.$router.back()
    }
  }
}
</script>

<style lang="less" scoped>
.search-container {
  padding-top: 108px;

  .van-search__action {
    color: #fff;
  }

  .search-form {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1;
  }
}
</style>
