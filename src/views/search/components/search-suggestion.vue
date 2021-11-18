<template>
  <div class="search-suggestion">
    <van-cell
      v-for="(item,index) in suggestions"
      :key.prop="index"
      :title="item"
      icon="search"
      @click="$emit('search', item)"
    >
      <template #title>
        <div v-html="heightLight (item)"></div>
      </template>
    </van-cell>
    <!--<div v-html="htmlStr"></div>-->
    <!--v-html可以渲染出标签-->
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
      timer: null,
      text: 123
    }
  },
  watch: {
    // 当searchText变化时，handler会执行，handler是固定写法
    searchText: {
      // 第一次没有触发，渲染完就触发事件
      immediate: true,
      handler: debounce(function (value) {
        this.loadSearchSuggestions(value)
      }, 500)
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
      } catch (err) {
        this.$toast('数据获取失败，请稍后重试')
      }
    },
    heightLight (text) {
      const highlightStr = `<span class="active">${this.searchText}</span>`
      // 正则表达式 // 中间的内容都会当作匹配字符来使用，而不是数据变量
      // 如果需要根据数据变量动态的创建正则表达式，则手动 new RegExp
      // RegExp 正则表达式构造函数
      //    参数1：匹配模式字符串，它会根据这个字符串创建正则对象
      //    参数2：匹配模式，要写到字符串中
      const reg = new RegExp(this.searchText, 'gi')
      return text.replace(reg, highlightStr)
    }
  }
}
</script>

<style lang="less" scoped>
.search-suggestion {
  /deep/ span.active {
    color: #3296fa;
  }
}
</style>
