<script setup>

  import { computed, onMounted } from 'vue'
  import { request } from './composables/useApi.js'
  import { useRoute } from 'vue-router';
  import SideBar from './components/SideBar.vue';

  onMounted(async () => {
    const data = await request('/matches')
    console.log(data)
  })

  const route = useRoute()

  const showSidebar = computed(() =>
    route.path !== '/login' && route.path !== '/register'
  )

</script>

<template>
  <SideBar v-if="showSidebar" />
  <main :class="showSidebar ? 'ml-48' : ''">
    <RouterView />
  </main>
</template>