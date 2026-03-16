<script setup>

    import { ref } from 'vue';
    import { RouterLink } from 'vue-router';
    import { useRouter } from 'vue-router'

    const router = useRouter()

    const isLoggedIn = ref(!!localStorage.getItem('token'))

    function logout() {
        localStorage.removeItem('token')
        isLoggedIn.value = false
        router.push('/ranking')
    }

</script>

<template>
  <nav class="fixed left-0 top-0 h-screen w-52 bg-gray-900 flex flex-col p-6 shadow-lg">
    <h2 class="text-white text-lg font-bold mb-8 border-b border-gray-700 pb-4 text-center">Metrolympiades</h2>

    <div class="flex flex-col gap-2 flex-1">
      <RouterLink 
        to="/ranking" 
        class="text-gray-300 hover:text-white hover:bg-gray-700 rounded-lg px-3 py-2 transition-colors"
      >
        🏆 Leaderboard
      </RouterLink>
      <RouterLink 
        v-if="isLoggedIn" 
        to="/team" 
        class="text-gray-300 hover:text-white hover:bg-gray-700 rounded-lg px-3 py-2 transition-colors"
      >
        👥 My team
      </RouterLink>
      <RouterLink 
        v-if="isLoggedIn" 
        to="/games" 
        class="text-gray-300 hover:text-white hover:bg-gray-700 rounded-lg px-3 py-2 transition-colors"
      >
        ⚽ My games
      </RouterLink>
      <RouterLink 
        v-if="!isLoggedIn" 
        to="/login" 
        class="text-gray-300 hover:text-white hover:bg-gray-700 rounded-lg px-3 py-2 transition-colors"
      >
        🔐 Login
      </RouterLink>
      <RouterLink 
        v-if="!isLoggedIn" 
        to="/register" 
        class="text-gray-300 hover:text-white hover:bg-gray-700 rounded-lg px-3 py-2 transition-colors"
      >
        📝 Register
      </RouterLink>
    </div>

    <button 
      v-if="isLoggedIn" 
      @click="logout" 
      class="text-gray-300 hover:text-white hover:bg-red-700 rounded-lg px-3 py-2 transition-colors text-left mt-auto"
    >
      🚪 Logout
    </button>
  </nav>
</template>