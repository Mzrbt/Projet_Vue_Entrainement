<script setup>

    import { getMyTeam, modMyTeam } from '../composables/useApi';
    import { onMounted, ref } from 'vue'
    import { useRouter } from 'vue-router'

    const router = useRouter()

    const name = ref("")
    const members = ref([])

    const isLoading = ref(false)

    const newMember = ref("")

    onMounted(async () => {
        isLoading.value = true
        const data = await getMyTeam()
        name.value = data.name
        members.value = data.members ?? []
        isLoading.value = false
    })

    function addMember() {
        if (newMember.value.trim()) {
            members.value.push(newMember.value.trim())
            newMember.value = ""
        }
    }

    function removeMember(index) {
        members.value.splice(index, 1)
    }   

    async function saveTeam() {

        isLoading.value = true

        try {
            await modMyTeam(name.value, members.value)
            window.alert("Équipe sauvegardée !")
        } catch (e) {
            window.alert("Erreur")
        } finally {
            isLoading.value = false
        }

    }

</script>

<template>
    <div class="p-10">
        <h1 class="text-4xl font-bold mb-8">Mon équipe</h1>

        <div class="bg-white border border-gray-200 rounded-lg p-6 shadow-sm max-w-lg flex flex-col gap-6">
            
            <div class="flex flex-col gap-2">
            <label class="font-semibold text-gray-700">Nom de l'équipe</label>
            <input 
                type="text" 
                v-model="name" 
                placeholder="Nom de l'équipe"
                class="border border-gray-300 rounded-lg px-3 py-2 focus:outline-none focus:border-blue-500"
            />
            </div>

            <div class="flex flex-col gap-2">
            <label class="font-semibold text-gray-700">Membres</label>
            <div v-if="members.length === 0" class="text-gray-400 text-sm">Aucun membre pour le moment</div>
            <div 
                v-for="(member, index) in members" 
                :key="index"
                class="flex items-center justify-between bg-gray-50 border border-gray-200 rounded-lg px-4 py-2"
            >
                <span>{{ member }}</span>
                <button 
                @click="removeMember(index)"
                class="text-red-400 hover:text-red-600 transition-colors"
                >✕</button>
            </div>

            <div class="flex gap-2 mt-2">
                <input 
                type="text" 
                v-model="newMember" 
                placeholder="Prénom du membre"
                class="flex-1 border border-gray-300 rounded-lg px-3 py-2 focus:outline-none focus:border-blue-500"
                />
                <button 
                @click="addMember"
                class="bg-blue-500 text-white px-4 py-2 rounded-lg hover:bg-blue-600 transition-colors"
                >Ajouter</button>
            </div>
            </div>

            <button @click="saveTeam" class="bg-green-500 text-white px-4 py-2 rounded-lg hover:bg-green-600 transition-colors">
            Sauvegarder
            </button>

        </div>
    </div>
</template>