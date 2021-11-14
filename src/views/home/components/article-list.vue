<template>
  <div>
    <van-list
      v-model="loading"
      :error.sync="error"
      error-text="请求失败，点击重新加载"
      @load="onLoad"
    >
      <van-cell v-for="item in list" :key="item" :title="item"/>
    </van-list>
  </div>
</template>

<script>

export default {
  name: 'article-list',
  data () {
    return {
      list: [],
      error: false,
      loading: false
    }
  },
  props: {
    channel: {
      type: Object,
      required: true
    }
  },
  created () {
    console.log(this.channel)
  },
  methods: {
    onLoad () {
      console.log(1)
    }
  }
}
</script>

<style scoped>

</style>
