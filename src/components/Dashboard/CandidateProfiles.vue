<template>
    <div class="bg-white p-8">
        <h2 class="text-xl font-bold mb-4">Candidate Profiles</h2>
        <div class="space-y-4">
            <div class="flex items-center rounded-lg border border-gray-200 py-2 px-4">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-500 mr-2" viewBox="0 0 20 20"
                    fill="currentColor">
                    <path fill-rule="evenodd"
                        d="M9 2a7 7 0 015.542 11.334l4.704 4.704a1 1 0 11-1.414 1.414l-4.704-4.704A7 7 0 119 2zm0 2a5 5 0 100 10A5 5 0 009 4z"
                        clip-rule="evenodd" />
                </svg>
                <input type="text" v-model="searchText" class="w-full outline-none text-gray-700"
                    placeholder="Search candidates...">
            </div>
            <div v-for="(candidate, index) in filteredCandidates" :key="index" class="bg-gray-100 p-4 rounded-lg">
                <div class="flex items-center justify-between mb-2">
                    <h3 class="text-lg font-medium">{{ candidate.name }}</h3>
                    <p class="text-sm">{{ candidate.stage }}</p>
                </div>
                <p class="text-gray-500">{{ candidate.jobTitle }}</p>
            </div>
        </div>
    </div>
</template>
  
<script setup lang="ts">
import { computed, ref } from 'vue';

const candidates = ref([
    {
        name: 'John Doe',
        jobTitle: 'Software Engineer',
        stage: 'Interviewing',
    },
    {
        name: 'Jane Smith',
        jobTitle: 'Product Manager',
        stage: 'Screening',
    },
    {
        name: 'Bob Johnson',
        jobTitle: 'Marketing Coordinator',
        stage: 'Sourcing',
    },
]);

const searchText = ref('')

const filteredCandidates = computed(() => {
    return candidates.value.filter(candidate => {
        return candidate.name.toLowerCase().includes(searchText.value.toLowerCase())
    })
})
</script>
  
<style scoped></style>
  