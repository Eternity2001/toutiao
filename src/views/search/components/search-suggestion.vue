<template>
  <div class="search-suggestion">
    <van-cell
      v-for="(item,index) in suggestions"
      :key.prop="index"
      :title="item"
      icon="search"
    ></van-cell>
  </div>
</template>

<script>
import { getSearchSuggestions } from '@/api/search'
import { debounce } from 'lodash'

export default {
  name: 'search-suggestion',
  components: {},
  props: {
    searchText: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      suggestions: [], // 联想建议数据列表
      htmlStr: 'Hello <span style="color: red">World</span>',
      timer: null
    }
  },
  watch: {
    // 当searchText变化时，handler会执行，handler是固定写法
    searchText: {
      // 第一次没有触发，渲染完就触发事件
      immediate: true,
      handler: debounce(function (value) {
        this.loadSearchSuggestions(value)
      }, 800)
      // handler (value) {
      //   this.loadSearchSuggestions(value)
      // }
    }
  },
  methods: {
    async loadSearchSuggestions (q) {
      try {
        const { data } = await getSearchSuggestions(q)
        this.suggestions = data.data.options
        console.log(data.data.options)
      } catch (err) {
        this.$toast('数据获取失败，请稍后重试')
      }
    }
  }
}
</script>

<style scoped>

</style>
