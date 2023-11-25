<template>
  <div class="h-4/5 p-4 overflow-y-auto">
    <div class="flex items-center gap-3">
      <div class="flex flex-col text-lg text-gray-700 p-3 w-full gap-3">
        <h1 class="font-mono font-semibold">Assessment Assistant App</h1>
        <p class="font-mono">
          Hello and welcome to the Assessment Assistant App! We're excited to help you evaluate your
          skills.
        </p>
        <p class="font-mono">
          Firstly provide a brief description of yourself and your area of the expertise.
        </p>
        <p class="font-mono">
          When you're ready to begin, simply click 'Start', and our friendly assistant will guide
          you through a series of questions designed to assess your abilities.
        </p>
        <p class="font-mono">Let's get started on your journey to success!</p>
      </div>
    </div>
    <div class="fixed left-0 bottom-0 w-full p-4 font-mono">
      <div class="flex flex-col gap-2">
        <div class="flex items-center">
          <textarea
            v-model="message"
            autofocus
            placeholder="Provide a brief description of yourself and your area of the expertise..."
            rows="3"
            class="w-full border border-gray-300 rounded-lg px-4 py-2"
          ></textarea>
        </div>
        <div class="flex items-center justify-end gap-2">
          <input
            type="email"
            v-model="email"
            @keyup.enter="start"
            class="flex-grow border border-gray-300 rounded-lg px-4 py-2"
            placeholder="Enter your email"
          />
          <button
            role="button"
            class="bg-blue-500 hover:bg-blue-600 text-white rounded-lg px-4 py-2"
            @click="start"
          >
            Start
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue'

const emit = defineEmits(['start'])
const message = ref('')
const email = ref('')

const start = () => {
  emit('start', {
    message: message.value,
    email: email.value
  })
}
</script>
