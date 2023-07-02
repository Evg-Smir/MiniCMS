<template>
    <div class="admin__inner">
        <adminSidebar @currentComponent="currentComponent"/>
        <div class="admin-view" :class="{'morePad': !stateAdminSidebar}">
            <Component :is="selectComponent" @currentComponent="currentComponent"/>
        </div>
    </div>
</template>

<script setup>
import { auth } from '~/server/setting'
import { useStore } from "~/store";
import adminSidebar from "~/components/admin/admin-sidebar.vue";

const store = useStore()
const siteInfo = computed(() => store.getSiteSetting)
const stateAdminSidebar = computed(() => useStore().getStateAdminSidebar)
const selectComponent = shallowRef('admin-main')
const reactiveStore = reactive(store.setting);

definePageMeta({
   layout: "admin",
   middleware: 'check-auth',
   auth: {auth}
});
useHead({
   title: `Админ`,
})

watch(() => reactiveStore, () => {
      useHead({
         title: `${siteInfo.value.name.param} : Админ`,
      })
   },
   {deep: true}
);

onMounted(() => {
   selectComponent.value = localStorage.getItem('currentComponentAdmin')
      ? localStorage.getItem('currentComponentAdmin')
      : 'admin-main'
})

const currentComponent = component => {
   localStorage.setItem('currentComponentAdmin', component.value);
   import(`~/components/admin/${component.value}.vue`)
      .then(val => {
         selectComponent.value = val.default
      })
}

</script>
