<script setup lang="ts">
import axios from 'axios'
import { nextTick, ref } from 'vue'

const apiKey = ref('')
let loading = ref(false)
const history = ref<{ role: string; content: string }[]>([])
const message = ref('')
const isTyping = ref(false)
const shouldScrollBottom = ref(false)
const lastTimeStamp = ref(0)
const timeLimit = ref(0)
const timer = ref(0);
const score = ref(0)

// @ts-ignore
let interval = null
const requestMessage = async (firstRequest: boolean) => {
  if (loading.value) return;

  isTyping.value = true
  loading.value = true

  if (message.value) {
    nextTick(() => {
      scrollMessagesContainerToBottom()
    })

    // @ts-ignore
    history.value.push({
      role: 'user',
      content: `${message.value}`
    })

    // @ts-ignore
    const differenceInMs = new Date().getTime() - lastTimeStamp.value
    history.value.push({
      role: 'system',
      content: `Time limit for question was: "${timeLimit.value} seconds"`
    });

    history.value.push({
      role: 'system',
      content: `Time taken for answer is: "${differenceInMs / 1000} seconds"`
    })

    message.value = ''
    score.value = 0
    clearTimers();
  }

  try {
    // request here
    const response = await axios.post(import.meta.env.VITE_ASSESSMENT_API_URL, {
      history: history.value,
      apiKey: apiKey.value,
      identifier: identifier.value,
      firstRequest
    });

    isTyping.value = false

    if (response.data.content) {
      nextTick(() => {
        if (!shouldScrollBottom.value) {
          shouldScrollBottom.value = true

          scrollMessagesContainerToTop()
          return
        }
        scrollMessagesContainerToBottom()
      })

      // @ts-ignore
      history.value.push({
        role: 'assistant',
        content: response.data.content || ''
      })

      timeLimit.value = response.data.timeLimit || 0
      timer.value = timeLimit.value;
      score.value = response.data.lastMessageScore || 0
      lastTimeStamp.value = new Date().getTime()

      if (timer.value > 0) {
        interval = setInterval(() => {
          timer.value--
          if (timer.value <= 0) {
            // @ts-ignore
            if (interval) {
              clearInterval(interval)

              history.value.push({
                role: 'system',
                content: `The candidate was not able to answer in proveded time limit: "${timeLimit.value} seconds". Say it for user and go next question."`
              })

              requestMessage(false)
            }
          }
        }, 1000)
      }
    }
  } catch (e) {
    console.error(e)
    alert(
      // @ts-ignore
      `Error: ${e?.message || 'Something went wrong'}\n\nTry to restart or contact ${import.meta.env.VITE_SUPPORT_EMAIL
      }`
    )
  }

  loading.value = false
}
const messagesContainer = ref<HTMLDivElement | null>(null)
const scrollMessagesContainerToBottom = () => {
  messagesContainer.value?.scrollTo({
    top: messagesContainer.value.scrollHeight,
    behavior: 'smooth'
  })
}

const scrollMessagesContainerToTop = () => {
  messagesContainer.value?.scrollTo({
    top: 0,
    behavior: 'smooth'
  })
}

const started = ref(false)

const identifier = ref()
const start = () => {
  if (!apiKey.value) {
    alert('Please provide api key')
  }

  identifier.value = new Date().getTime()

  started.value = true

  requestMessage(true)
}

function formatTime(time: number) {
  let minutes = Math.floor(time / 60)
  let seconds = Math.abs(time % 60)

  if (minutes < 0) minutes = 0
  if (seconds <= 1) seconds = 0

  const minutesStr = minutes < 10 ? `0${minutes}` : `${minutes}`
  const secondsStr = seconds < 10 ? `0${seconds}` : `${seconds}`
  return `${minutesStr}:${secondsStr}`
}

const clearTimers = () => {
  // @ts-ignore
  if (interval) clearInterval(interval)
  timeLimit.value = 0
  timer.value = 0
}
</script>

<template>
  <main>
    <div class="flex flex-col h-screen">
      <!-- Chat messages container -->
      <div v-if="started" ref="messagesContainer" class="h-4/5 p-4 overflow-y-auto">
        <TransitionGroup name="list" tag="div" class="flex flex-col gap-4">
          <div v-for="msg in history.filter((m) => m.role === 'user' || m.role === 'assistant')" :key="msg.content"
            class="flex" :class="{
              'justify-start': msg.role === 'assistant',
              'justify-end': msg.role === 'user'
            }">
            <div class="rounded-lg p-3 max-w-md" :class="{
              'bg-gray-200 text-gray-700 ': msg.role === 'assistant',
              'bg-blue-500 text-white ': msg.role === 'user'
            }">
              <pre class="whitespace-pre-wrap font-serif">{{ msg.content }}</pre>
            </div>
          </div>
        </TransitionGroup>
      </div>
      <div v-else class="h-4/5 p-4 overflow-y-auto">
        <div class="flex items-center gap-3">
          <div class="text-gray-700 p-3 w-full">
            <h1 class="font-mono font-semibold">Assessment Assistant App</h1>
            <p class="font-mono">
              Hello and welcome to the Assessment Assistant App! We're excited to help you evaluate
              your skills. When you're ready to begin, simply click 'Start', and our friendly
              assistant will guide you through a series of questions designed to assess your
              abilities. Let's get started on your journey to success!
            </p>
          </div>
        </div>
      </div>

      <!-- Chat input container -->
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
            <textarea v-if="started" @keyup.enter="
              ($event) => {
                if ($event.altKey || $event.ctrlKey || $event.shiftKey) {
                  return
                }

                requestMessage(false)
              }
            " v-model="message" placeholder="Type your message..." rows="3"
              class="flex-grow border border-gray-300 rounded-lg px-4 py-2"></textarea>
            <input v-if="!started" type="password" v-model="apiKey" @keyup.enter="
              () => {
                start()
              }" class="flex-grow border border-gray-300 rounded-lg px-4 py-2" placeholder="Insert Open API key" />
          </div>
          <div class="flex items-center gap-2" :class="{
            'justify-between': started,
            'justify-end': !started
          }">
            <button v-if="started" :disabled="loading"
              class="bg-blue-500 hover:bg-blue-600 text-white rounded-lg px-4 py-2" @click="
                () => {
                  loading = false
                  started = false
                  history = []
                  message = ''
                  clearTimers()
                }
              ">
              Restart
            </button>
            <div v-if="started" class="flex justify-center items-center rounded-lg bg-indigo-100 w-32 h-10">
              <span class="text-2xl font-bold">{{ formatTime(timer) }}</span>
            </div>
            <div v-if="started" class="flex justify-center items-center rounded-lg bg-indigo-100 w-32 h-10">
              <span class="text-2xl font-bold">{{ score }}</span>
            </div>
            <button role="button" :disabled="loading"
              class="bg-blue-500 hover:bg-blue-600 text-white rounded-lg px-4 py-2" @click="
                () => {
                  !started ? start() : requestMessage(false)
                }
              ">
              {{ !started ? 'Start' : 'Send' }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>
<style postcss scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}

.typing-indicator span {
  animation: pulse 1s ease-in-out infinite;
}

@keyframes pulse {

  0%,
  100% {
    opacity: 1;
  }

  50% {
    opacity: 0.5;
  }
}

.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
</style>
