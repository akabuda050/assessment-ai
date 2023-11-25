<template>
    <div class="bg-white p-8">
      <h2 class="text-xl font-bold mb-4">Employee Pipeline Overview</h2>
      <div class="flex justify-center">
        <CandidateStageCard stage="Sourcing" :count="sourcing" />
        <CandidateStageCard stage="Screening" :count="screening" />
        <CandidateStageCard stage="Interviewing" :count="interviewing" />
        <CandidateStageCard stage="Assessing" :count="hiring" />
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import CandidateStageCard from './CandidateStageCard.vue'
  import { defineProps } from 'vue'
  
  const props = defineProps({
    sourcing: {
      type: Number,
      required: true,
      default: 0,
    },
    screening: {
      type: Number,
      required: true,
      default: 0,
    },
    interviewing: {
      type: Number,
      required: true,
      default: 0,
    },
    hiring: {
      type: Number,
      required: true,
      default: 0,
    },
  })
  </script>
  
  <style scoped></style>
  