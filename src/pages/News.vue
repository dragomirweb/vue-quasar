<template>
  <div class="q-pa-md row q-gutter-md">
    <q-card v-for="article in newsArticles" v-bind:key="article" class="my-card flex flex-column bg-secondary text-white">
      <q-card-section>
        <div class="text-h6">{{ article.title }}</div>
        <div class="text-subtitle2">{{ article.by }}</div>
      </q-card-section>

      <q-separator dark />

      <q-card-actions class="mt-auto ml-auto">
        <q-btn type="a" target="blank" flat :href="article.url">Link</q-btn>
      </q-card-actions>
    </q-card>
  </div>
</template>
<script>
import axios from 'axios'

export default {
  name: "Reddit",
  data() {
    return {
      newsArticles: [],
      newsUrl: 'https://hacker-news.firebaseio.com/v0/topstories.json',
      articleUrl: 'https://hacker-news.firebaseio.com/v0/item/'
    };
  },
  mounted () {
    axios
      .get(this.newsUrl)
      .then(r => r.data.forEach((el, index) => {
       if(index < 30) {
          axios
          .get(this.articleUrl + `${el}.json`)
          .then(r => this.newsArticles.push(r.data))
       }
      }))
  }
};
</script>
<style lang="stylus">
  .my-card
    max-width calc(25% - 16px)
    min-width calc(25% - 16px)
    @media (max-width: 991px)
      max-width calc(50% - 16px)
      min-width calc(50% - 16px)
    @media (max-width: 500px)
      max-width calc(100% - 16px)
      min-width calc(100% - 16px)  
  .mt-auto
    margin-top auto
  .ml-auto
    margin-left auto
</style>