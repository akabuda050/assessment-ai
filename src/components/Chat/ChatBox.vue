<template>
    <div class="flex flex-col h-screen bg-gray-100">
        <div class="flex-1 p-4 overflow-y-scroll">
            <transition-group name="message" tag="div" class="grid gap-4">
                <div v-for="message in messages" :key="message.id" class="flex items-center">
                    <div
                        :class="['flex items-center p-2 rounded-lg shadow-sm', message.isMe ? 'bg-blue-500 text-white ml-auto' : 'bg-gray-300 mr-auto']">
                        <div class="text-sm">{{ message.content }}</div>
                        <div class="text-xs ml-2 text-gray-500">{{ formatTime(message.time) }}</div>
                    </div>
                </div>
            </transition-group>
            <div ref="messagesEnd"></div>
        </div>
        <div class="flex-none p-4">
            <form class="flex items-center" @submit.prevent="sendMessage">
                <input class="flex-1 px-4 py-2 mr-2 text-sm bg-gray-200 rounded-full focus:outline-none"
                    v-model="newMessage" type="text" placeholder="Type a message...">
                <button class="px-4 py-2 text-white rounded-full shadow-md hover:shadow-lg focus:outline-none"
                    :class="newMessage ? 'bg-blue-500' : 'bg-gray-300'" type="submit" :disabled="!newMessage">Send</button>
            </form>
        </div>
    </div>
</template>
  
<script setup lang="ts">
import { ref, onMounted } from 'vue'

const messages = ref([
    { id: 1, author: 'Jane', content: 'Hi, how are you?', isMe: true, time: '10:01am' },
    { id: 2, author: 'John', content: 'I\'m good, thanks. How about you?', isMe: true, time: '10:02am' },
    { id: 3, author: 'Jane', content: 'I\'m doing well, thanks for asking.', isMe: true, time: '10:03am' }
])

const newMessage = ref('')

function sendMessage() {
    messages.value.push({
        id: messages.value.length + 1,
        author: 'Me',
        content: newMessage.value,
        isMe: true,
        time: new Date().toISOString()
    })
    newMessage.value = ''
}

function formatTime(time: string) {
    const date = new Date(time)
    const now = new Date()
    if (now.getFullYear() === date.getFullYear() && now.getMonth() === date.getMonth() && now.getDate() === date.getDate()) {
        return date.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
    } else {
        return date.toLocaleDateString([], { month: 'short', day: 'numeric' })
    }
}

// Scroll to bottom of messages when new message is added
onMounted(() => {
    const messagesEnd = ref(null)
    const scrollToBottom = () => {
        messagesEnd.value.scrollIntoView({ behavior: 'smooth' })
    }
    scrollToBottom()
})
</script>
  