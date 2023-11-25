<template>
    <div class="bg-white p-8">
        <div class="flex justify-between">
            <div class="text-center">
                <p class="text-gray-500 text-sm uppercase font-medium mb-2">Candidates in pipeline</p>
                <p class="text-3xl font-bold">{{ candidates }}{{ candidatesUnit }}</p>
            </div>
            <div class="text-center">
                <p class="text-gray-500 text-sm uppercase font-medium mb-2">Interviews scheduled</p>
                <p class="text-3xl font-bold">{{ interviews }}{{ interviewsUnit }}</p>
            </div>
            <div class="text-center">
                <p class="text-gray-500 text-sm uppercase font-medium mb-2">Time to assess</p>
                <p class="text-3xl font-bold">{{ timeToHire }}</p>
            </div>
        </div>
    </div>
</template>
  
<script setup lang="ts">

    defineProps({
    candidates: {
        type: Number,
            required: true,
        default: 0,
      },
    candidatesUnit: {
        type: String,
            required: false,
        default: '',
      },
    interviews: {
        type: Number,
            required: true,
        default: 0,
      },
    interviewsUnit: {
        type: String,
            required: false,
        default: '',
      },
    timeToHire: {
        type: String,
            required: true,
        default: '0 days',
      },
});
</script>
  
<style scoped></style>
  