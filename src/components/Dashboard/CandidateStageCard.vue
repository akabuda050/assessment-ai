<template>
    <div class="w-1/4 mr-4">
      <div class="flex items-center justify-between mb-4">
        <p class="text-lg font-medium">{{ stage }}</p>
      </div>
      <div class="flex items-center justify-center bg-gray-100 rounded-lg h-48 mb-4">
        <p class="text-center text-lg py-8 font-medium">{{ count }} employees</p>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { defineProps } from 'vue'
  
  const props = defineProps({
    stage: {
      type: String,
      required: true,
    },
    count: {
      type: Number,
      required: true,
      default: 0,
    },
  })
  </script>
  
  <style scoped></style>
  