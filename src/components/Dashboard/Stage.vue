<template>
    <div class="w-1/4 mr-4">
        <div class="flex items-center justify-between mb-4">
            <p class="text-lg font-medium capitalize">{{ stage }}</p>
            <p class="text-lg">{{ candidates.length }}</p>
        </div>
        <div class="bg-gray-100 rounded-lg h-48 min-h-48 mb-4 overflow-y-auto">
            <ul>
                <li v-for="candidate in candidates" :key="candidate.id" class="p-4 border-b border-gray-200">
                    <p class="font-medium">{{ candidate.name }}</p>
                    <p class="text-gray-500">{{ candidate.email }}</p>
                </li>
            </ul>
        </div>
    </div>
</template>
  
<script setup lang="ts">
import type { PropType } from 'vue'
import type { Candidate } from '@/types/Candidate'

defineProps({
    candidates: {
        type: Array as PropType<Candidate[]>,
        required: true,
    },
    stage: {
        type: String,
        required: true,
    }
})
</script>
  
<style scoped></style>
  