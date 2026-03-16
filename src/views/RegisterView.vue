<script setup>

    import { ref, computed } from 'vue'
    import { useRouter } from 'vue-router'
    import { register } from '../composables/useApi';

    const router = useRouter()

    const username = ref("")
    const teamName = ref("")
    const email = ref("")
    const password = ref("")
    
    const emailError = ref("")

    const isFormValid = computed(() => {
        return username.value.trim() && teamName.value.trim() && email.value.trim() && password.value.trim()
    })

    const isLoading = ref(false)

    function validateEmail() {
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
        emailError.value = emailRegex.test(email.value) ? '' : 'Veuillez entrer un email valide'
    }

    async function handleRegister() {
        
        isLoading.value = true

        try {
            const data = await register(email.value, username.value, password.value, teamName.value)
            localStorage.setItem("token", data.token)
            router.push('/ranking')
        } catch (e) {
            window.alert("Informations incorrects")
            password.value = ''
        } finally {
            isLoading.value = false
        }
    }

</script>

<template>
    <form class="flex flex-col items-center" @submit.prevent="handleRegister">
        <h1 class="text-5xl m-15">Register</h1>
        <p v-if="emailError" class="text-red-500 text-sm">{{ emailError }}</p>
        <input
            type="text"
            id="username"
            name="username"
            autocomplete="username"
            required
            v-model="username"
            placeholder="Username"
            class="m-3 border border-black px-2"
        />
        <input
            type="text"
            id="teamname"
            name="teamname"
            autocomplete="teamname"
            required
            v-model="teamName"
            placeholder="Teamname"
            class="m-3 border border-black px-2"
        />
        <input
            type="email"
            id="email"
            name="email"
            autocomplete="email"
            required
            v-model="email"
            placeholder="Email"
            class="m-3 border border-black px-2"
            @blur="validateEmail"
        />
        <input
            type="password"
            id="password"
            name="password"
            autocomplete="current-password"
            required
            v-model="password"
            placeholder="Password"
            class="m-3 border border-black px-2"
        />
        <button type="submit" class="mt-10 px-6 py-2 bg-blue-500 text-white rounded cursor-pointer hover:bg-blue-600 disabled:opacity-50 disabled:cursor-not-allowed disabled:hover:bg-blue-500" :disabled="!isFormValid || isLoading" :class="{ loading: isLoading }">
            Register
        </button>
        <router-link to="/login" class="mt-5 text-blue-500 hover:underline">Already have an account ?</router-link>
    </form>
</template>