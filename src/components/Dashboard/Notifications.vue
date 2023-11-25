<template>
    <div class="bg-white rounded-lg shadow-md p-4">
        <div class="flex justify-between items-center mb-4">
            <h2 class="text-xl font-bold mb-4">Notifications {{ unreadCount }}</h2>
        </div>
        <div v-if="notifications.length === 0" class="text-gray-500 text-center">
            No notifications
        </div>
        <div v-else>
            <div v-for="(notification, index) in notifications" :key="index"
                class="mb-4 cursor-pointer hover:bg-gray-100 rounded-lg py-3 px-4"
                :class="{ 'bg-blue-300': !notification.read }" @click="markAsRead(index)">
                <div class="flex items-center">
                    <div
                        :class="['rounded-full h-8 w-8 flex items-center justify-center mr-4', { 'bg-blue-200': !notification.read, 'bg-gray-200': notification.read }]">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-600" viewBox="0 0 20 20"
                            fill="currentColor">
                            <path fill-rule="evenodd"
                                d="M10 18a2 2 0 100-4 2 2 0 000 4zM16 8a6 6 0 11-12 0 6 6 0 0112 0zM10 0a1.25 1.25 0 00-1.25 1.25v.25a1.25 1.25 0 102.5 0v-.25A1.25 1.25 0 0010 0z"
                                clip-rule="evenodd" />
                        </svg>
                    </div>
                    <div>
                        <div class="font-semibold">{{ notification.title }}</div>
                        <div class="text-gray-500 text-sm">{{ notification.date }}</div>
                    </div>
                </div>
                <div class="text-gray-500 text-sm mt-2">{{ notification.description }}</div>
            </div>
            <div class="mt-4 flex justify-center">
                <button class="text-blue-500 hover:text-blue-700 font-medium text-sm focus:outline-none"
                    @click="markAllAsRead">Mark all as read</button>
            </div>
        </div>
    </div>
</template>
  
<script setup lang="ts">
import { ref } from 'vue';

const notifications = ref([
    {
        title: 'New candidate',
        date: 'Apr 11, 2023',
        description: 'John Doe applied for the Frontend Developer position',
        read: false
    },
    {
        title: 'Upcoming interview',
        date: 'Apr 13, 2023',
        description: 'You have an interview with Jane Smith scheduled for 10:00 AM',
        read: true
    },
    {
        title: 'Recruitment event',
        date: 'Apr 14, 2023',
        description: 'The Recruitment Fair will take place from 2:00 PM to 5:00 PM',
        read: true
    }
])

const unreadCount = notifications.value.filter(n => !n.read).length

function markAsRead(index: number) {
    notifications.value[index].read = true
}
function markAllAsRead() {
    notifications.value.forEach(n => n.read = true)
}
</script>