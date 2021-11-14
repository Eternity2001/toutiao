<template>
  <van-cell
    class="article-item"
  >
    <div slot="title" class="title">{{ article.title }}</div>
    <div slot="label">
      <div v-if="article.cover.type === 3" class="cover-wrap">
        <div
          v-for="(img, index) in article.cover.images"
          :key="index"
          class="cover-item"
        >
          <van-image
            :src="img"
            height="100"
            width="100"
          />
        </div>
      </div>
      <div>
        <span>{{ article.aut_name }}</span>
        <span>{{ article.comm_count }}评论</span>
        <span>{{ article.pubdate }}</span>
      </div>
    </div>
    <van-image
      v-if="article.cover.type === 1"
      slot="default"
      :src="article.cover.images[0]"
      height="100"
      width="100"
    />
  </van-cell>
</template>

<script>
export default {
  name: 'ArticleItem',
  components: {},
  props: {
    article: {
      type: Object,
      required: true
    }
  },
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {
  },
  mounted () {
  },
  methods: {}
}
</script>

<style lang="less" scoped></style>
