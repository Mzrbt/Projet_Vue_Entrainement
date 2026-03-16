<script setup>

    import { computed } from 'vue';
    import { RouterLink } from 'vue-router';
    import { useRouter } from 'vue-router'

    const router = useRouter()

    const isLoggedIn = computed(() => !!localStorage.getItem('token'))

    function logout() {
        localStorage.removeItem('token')
        router.push('/ranking')
    }

</script>

<template>
    <nav class="fixed left-0 top-0 h-screen w-48 bg-gray-800 flex flex-col p-4">
        
        <RouterLink to="/ranking">Leaderboard</RouterLink>

        <RouterLink v-if="isLoggedIn" to="/team">My team</RouterLink>
        <RouterLink v-if="isLoggedIn" to="/games">My games</RouterLink>
        <button v-if="isLoggedIn" @click="logout">Logout</button>

        <RouterLink v-if="!isLoggedIn" to="/login">Login</RouterLink>
        <RouterLink v-if="!isLoggedIn" to="/register">Register</RouterLink>

    </nav>
</template>