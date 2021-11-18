<template>
  <div class="search-container">
    <!-- 搜索框 -->
    <form action="/" class="search-form">
      <van-search
        v-model="searchText"
        autofocus
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
    <search-result
      v-if="isResultShow"
      :search-text="searchText"
    />
    <!-- /搜索结果 -->

    <!-- 联想建议 -->
    <search-suggestion
      v-else-if="searchText"
      :search-text="searchText"
      @search="onSearch"
    />
    <!-- /联想建议 -->

    <!-- 历史记录 -->
    <search-history
      v-else
      :searchHistories="searchHistories"
      @search="onSearch"
      @clear-search-histories="clearSearchHistories"
    />
    <!-- /历史记录 -->
  </div>
</template>

<script>
import SearchHistory from './components/search-history'
import SearchResult from './components/search-result'
import SearchSuggestion from './components/search-suggestion'
import { getItem, setItem } from '@/utils/storage'
import { Dialog } from 'vant'

export default {
  name: 'SearchIndex',
  components: {
    SearchHistory,
    SearchResult,
    SearchSuggestion
  },
  data () {
    return {
      searchText: '',
      isResultShow: false, // 控制搜索结果的展示
      searchHistories: getItem('TOUTIAO_SEARCH_HISTORIES') || [] // 搜索的历史记录数据
    }
  },
  methods: {
    onSearch (val) {
      this.searchText = val

      const index = this.searchHistories.indexOf(val)
      if (index !== -1) {
        this.searchHistories.splice(index, 1)
      }
      this.searchHistories.unshift(val)
      this.isResultShow = true
    },
    onCancel () {
      this.$router.back()
    },
    // 自定义删除提示
    clearSearchHistories () {
      Dialog.confirm({
        message: '请确认是否删除历史记录'
      }).then(() => {
        this.searchHistories = []
      })
    }
    // 自定义删除提示
  },
  watch: {
    searchHistories (value) {
      console.log(value)
      setItem('TOUTIAO_SEARCH_HISTORIES', value)
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
