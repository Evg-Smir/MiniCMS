<template>
    <div class="site">
        <AppHeader/>
        <slot/>
        <AppFooter/>
        <loader/>
    </div>
</template>

<script setup>
import AppHeader from "~/components/header/AppHeader.vue";
import AppFooter from "~/components/footer/AppFooter.vue";
import { useStore } from "~/store";
import loader from "~/components/preloader.vue";

const store = useStore()

onMounted(() => {
   store.fetchSiteSetting()
   store.fetchArticleCategories()
   store.fetchArticlesList()
   store.fetchEventsList()
   store.fetchFriendsList()
})
</script>
