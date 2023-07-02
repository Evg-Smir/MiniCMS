<template>
    <section class="news-page">
        <div class="wrapper">
            <h2 class="news-page__title" v-text="this.news.name"/>
            <div v-html="news.content"></div>
        </div>
    </section>
</template>

<script>
import { useStore } from "~/store";

export default {
   data() {
      return {
         news: {},
      }
   },
   computed: {},
   mounted() {
      const store = useStore()
      const routeParamsNews = this.$route.params.news;
      const storedNews = JSON.parse(localStorage.getItem('currentNews'));
      const articleFromStore = store.getArticle(routeParamsNews);

      if (articleFromStore) {
         this.news = articleFromStore;
      } else if (storedNews) {
         this.news = storedNews;
      }

      localStorage.setItem('currentNews', JSON.stringify(this.news));
   },
}
</script>
