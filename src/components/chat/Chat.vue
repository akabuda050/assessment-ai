<template>
  <div ref="messagesContainer" class="h-4/5 p-4 overflow-y-auto">
    <TransitionGroup name="list" tag="div" class="flex flex-col gap-4">
      <div
        v-for="message in props.history.filter((m) => m.role === 'user' || m.role === 'assistant')"
        :key="message.content"
        class="flex"
        :class="{
          'justify-start': message.role === 'assistant',
          'justify-end': message.role === 'user'
        }"
      >
        <div
          class="rounded-lg p-3 max-w-md"
          :class="{
            'bg-gray-200 text-gray-700 ': message.role === 'assistant',
            'bg-blue-500 text-white ': message.role === 'user'
          }"
        >
          <pre class="whitespace-pre-wrap font-serif">{{ message.content }}</pre>
        </div>
      </div>
    </TransitionGroup>
    <div class="fixed bottom-0 w-full p-4 font-mono">
      <div class="flex flex-col gap-2">
        <transition name="fade">
          <div v-if="isTyping" class="typing-indicator flex justify-start pl-1">
            <span class="h-2 w-2 rounded-full bg-gray-400 mr-1"></span>
            <span class="h-2 w-2 rounded-full bg-gray-400 mr-1"></span>
            <span class="h-2 w-2 rounded-full bg-gray-400"></span>
          </div>
        </transition>
        <div class="flex items-center">
          <textarea
            v-if="started"
            @keyup.enter="
              ($event) => {
                if ($event.altKey || $event.ctrlKey || $event.shiftKey) {
                  return
                }

                requestMessage(false)
              }
            "
            v-model="message"
            placeholder="Type your message..."
            rows="3"
            class="flex-grow border border-gray-300 rounded-lg px-4 py-2"
          ></textarea>
          <input
            v-if="!started"
            type="password"
            v-model="apiKey"
            @keyup.enter="
              () => {
                start()
              }
            "
            class="flex-grow border border-gray-300 rounded-lg px-4 py-2"
            placeholder="Insert api key"
          />
        </div>
        <div
          class="flex items-center gap-2"
          :class="{
            'justify-between': started,
            'justify-end': !started
          }"
        >
          <button
            v-if="started"
            :disabled="loading"
            class="bg-blue-500 hover:bg-blue-600 text-white rounded-lg px-4 py-2"
            @click="
              () => {
                loading = false
                started = false
                history = []
                message = ''
                clearTimers()
              }
            "
          >
            Restart
          </button>
          <div
            v-if="started"
            class="flex justify-center items-center rounded-lg bg-indigo-100 w-32 h-10"
          >
            <span class="text-2xl font-bold">{{ formatTime(timer) }}</span>
          </div>
          <div
            v-if="started"
            class="flex justify-center items-center rounded-lg bg-indigo-100 w-32 h-10"
          >
            <span class="text-2xl font-bold">{{ score }}</span>
          </div>
          <button
            role="button"
            :disabled="loading"
            class="bg-blue-500 hover:bg-blue-600 text-white rounded-lg px-4 py-2"
            @click="
              () => {
                !started ? start() : requestMessage(false)
              }
            "
          >
            {{ !started ? 'Start' : 'Send' }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import type { PropType } from 'vue'

const props = defineProps({
  history: {
    type: Array as PropType<{ role: string; content: string }[]>,
    required: true
  },
  loading: {
    type: Boolean,
    default: false
  }
})
</script>
