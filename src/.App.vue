<template>
  <div class="flex flex-col h-screen">
    <WelcomeScreen v-if="!started" @start="onStart" />
    <Chat :history="chatHistory" />
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue'
import WelcomeScreen from './components/base/WelcomeScreen.vue'
import Chat from './components/chat/Chat.vue'
const started = ref(false)
const chatHistory = ref<{ role: string; content: string }[]>([])
const onStart = ({ email, message }: { email: string; message: string }) => {
  chatHistory.value.push({
    role: 'user',
    content: message
  })

  started.value = true

  console.log(`Interview session started for user: "${email}"`)
}
</script>
