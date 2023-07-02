<template>
    <div class="main-section">
        <div class="wrapper">
            <NewsSection v-if="settingPageIndex.newsList.state"/>
            <EventsSections v-if="settingPageIndex.calendar.state"/>
        </div>
    </div>
</template>

<script setup>
import { useStore } from "~/store";
import { state } from "~/store/state";
import NewsSection from "~/components/sections/news-section.vue";
import EventsSections from "~/components/sections/events-sections.vue";

const store = useStore()
const siteInfo = computed(() => store.getSiteSetting)
const settingPageIndex = computed(() => state().getIndexPageSetting)
const reactiveStore = reactive(store.setting);

definePageMeta({
   layout: "default",
});
useHead({
   title: `${!!siteInfo.value.name ? siteInfo.value.name.param : ''}`,
})

watch(() => reactiveStore, () => {
      useHead({
         title: `${siteInfo.value.name.param}`,
      })
   },
   {deep: true}
);

</script>
