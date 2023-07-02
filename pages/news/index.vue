<template>
    <section class="news-page page">
        <div class="wrapper">
            <h1 class="page-title">Новости</h1>
            <div class="page__filter">
                <label>
                    <input class="input" type="text" placeholder="Найти новость..." v-model="searchNews">
                    <v-icon v-if="showClearInput" icon="mdi-close" @click="clearSearchInput"/>
                </label>
            </div>
            <div v-if="!listNews.length">
                Идет загрузка
            </div>
            <div v-else-if="listNews.length > 0" class="news-content">
                <TransitionGroup v-if="filteredListNews.length > 0" ke name="fade" tag="div" class="news__list">
                    <nuxt-link
                      v-for="news in filteredListNews"
                      :to="`/news/${news.id}`"
                      class="news-item"
                      :key="news.id"
                    >
                        <div class="news-preview">
                            <img :src="news.preview" alt="Preview">
                        </div>
                        <div class="news-info">
                            <div class="info__category" v-text="category(news.category)?.name"/>
                            <div class="info__name" v-text="news.name"/>
                            <div class="info__description" v-html="news.content"></div>
                            <div class="info__date" v-text="news.date"/>
                        </div>
                    </nuxt-link>
                </TransitionGroup>
                <div
                  v-else
                  class="news__nothing"
                >
                    По Вашему запросу ничего не найдено
                </div>
            </div>
            <div v-else-if="!listNews.length">
                К сожалению пока нет новостей
            </div>
        </div>
    </section>
</template>

<script setup>
import { useStore } from "~/store";
import debounce from "~/utils/debounce";

definePageMeta({
   layout: "default",
});
useHead({
   title: `Новости`,
})

const searchNews = ref('')
const showClearInput = ref(false)

const listNews = computed(() => {
   return useStore().getArticles
})

const listCategories = computed(() => {
   return useStore().getCategories
})

const filteredListNews = computed(() => {
   return listNews.value.filter(news => {
      return news.name.toLowerCase().includes(searchNews.value.toLowerCase()) && news.status
   })
})

const category = index => {
   return listCategories.value.find(item => item.id === index)
}
const clearSearchInput = () => {
   searchNews.value = ''
}

watch(() => searchNews, () => {
   showClearInput.value = searchNews.value.trim().length
})
</script>

<style scoped>
.fade-move,
.fade-enter-active,
.fade-leave-active {
    transition: all 0.5s cubic-bezier(0.55, 0, 0.1, 1);
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
    transform: scaleY(0.01) translate(30px, 0);
}

.fade-leave-active {
    position: absolute;
    z-index: 9;
}
</style>
