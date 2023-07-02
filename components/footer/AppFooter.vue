<template>
    <section class="footer-section">
        <div class="wrapper">
            <div class="footer-section__inner">
                <div class="footer__block">
                    <div class="footer__block__logo">
                        <div class="logo__image">
                            <img src="../../assets/images/icon.png" alt="">
                        </div>
                        <div class="logo__text" v-text="name"/>
                    </div>
                    <div class="footer__block__info">
                        {{ getYear }} г. © {{ name }}
                    </div>
                </div>
                <div class="footer__block">
                    <div class="footer__block__links">
                        <a v-if="phone" :href="`tel:${phone}`" v-text="phone"/>
                    </div>
                    <div class="footer__block__links">
                        ул. Строителей, 40/1, Магнитогорск, Челябинская обл., 455000
                    </div>
                    <div class="footer__block__links" v-if="false">
                        l1 l2 l3
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<script setup>

import { useStore } from "~/store";

const store = useStore()

const getYear = computed(() => (new Date()).getFullYear())
const phone = computed(() => store.getSiteSettingParam('phone')?.param)
const name = computed(() => store.getSiteSettingParam('name')?.param)

</script>
