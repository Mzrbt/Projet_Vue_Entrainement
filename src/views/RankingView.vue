<script setup>

    import { ref, computed } from 'vue'
    import { useRouter } from 'vue-router'
    import { ranking } from '../composables/useApi.js'
    import SideBar from '../components/SideBar.vue';

    const router = useRouter()



</script>

<template>

    <SideBar/>

</template>