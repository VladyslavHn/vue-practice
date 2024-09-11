<template>
  <div class="container pt-1">
    <div class="card">
      <h2>News {{ now }}</h2>
    </div>

    <app-news 
    v-for="(item, index) in news" 
    :key="index"
    :title="item.title"
    :summary="item.summary">
    </app-news>

  </div>
</template>

<script>
import AppNews from './components/AppNews.vue'
import newsData from './News.json'

export default {
  data () {
    return {
      now: new Date().toLocaleDateString(),
      news: newsData.news
    }
  },
  components: {
    'app-news': AppNews
  }
}
</script>

<style lang="scss">
</style>
