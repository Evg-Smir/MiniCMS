<template>
    <section class="admin">
        <slot/>
        <EditedModal/>
        <adminFriendModal/>
        <loader/>
    </section>
</template>

<script setup>
import EditedModal from "~/components/EditedModal.vue";
import adminFriendModal from "~/components/admin/admin-friend-modal.vue";
import loader from "~/components/preloader.vue";
import { useStore } from "~/store";

const store = useStore()

onMounted(() => {
   store.fetchSiteSetting()
   store.fetchArticleCategories()
   store.fetchArticlesList()
   store.fetchEventsList()
   store.fetchFriendsList()
})
</script>
