<template>
    <section class="header-section">
        <div class="wrapper">
            <div class="header-section__inner">
                <NuxtLink :to="'/'" class="header-section__logo">
                    <img src="../../assets/images/icon.png" alt="">
                    <div v-if="windowWidth <= 768" class="header-section__mobile">
                        <span v-text="name"/>
                    </div>
                </NuxtLink>
                <AppHeaderList v-if="windowWidth > 768"/>
                <div v-if="windowWidth > 768" class="header-section__phone">
                    <a :href="`tel:${phone}`" v-text="phone"/>
                </div>
                <AppHeaderBurger v-if="windowWidth <= 768"/>
            </div>
        </div>
    </section>
</template>

<script setup>
import AppHeaderBurger from "~/components/header/AppHeaderBurger.vue"
import AppHeaderList from "~/components/header/AppHeaderList.vue";
import { useStore } from "~/store";

const store = useStore()
const phone = computed(() => store.getSiteSettingParam('phone')?.param)
const name = computed(() => store.getSiteSettingParam('name')?.param)

const windowWidth = ref(window.innerWidth);
const handleResize = () => windowWidth.value = window.innerWidth;

onMounted(() => {
   window.addEventListener('resize', handleResize);
})

</script>
