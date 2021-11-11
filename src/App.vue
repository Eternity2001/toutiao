<template>
  <div id="#app">
    <h1><i class="toutiao icon-shoucang"></i>黑马头条</h1>
    <h2>一剑独尊</h2>
    <van-button type="primary">主要按钮</van-button>
    <van-button type="info">信息按钮</van-button>
    <van-button type="default">默认按钮</van-button>
    <van-button type="warning">警告按钮</van-button>
    <van-button type="danger">危险按钮</van-button>
    <van-cell center title="标题">
      <template #right-icon>
        <van-switch v-model="checked" size="24"/>
      </template>
    </van-cell>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      checked: true
    }
  }
}
</script>

<style scoped>
</style>
