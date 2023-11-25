<template>
  <div class="bg-white p-6 rounded-md shadow-lg">
    <h2 class="text-3xl font-bold text-gray-800 mb-4">Personal Information</h2>
    <form @submit.prevent="submitForm" class="space-y-6">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div>
          <label class="block text-gray-700 font-bold mb-1 text-lg" for="name">
            Name <span class="text-red-600">*</span>
          </label>
          <input
            class="appearance-none bg-gray-100 border-2 border-gray-200 rounded-md w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-blue-500 focus:shadow-outline-blue"
            id="name"
            type="text"
            v-model="personalInfo.name"
            required
          />
        </div>
        <div>
          <label class="block text-gray-700 font-bold mb-1 text-lg" for="email">
            Email <span class="text-red-600">*</span>
          </label>
          <input
            class="appearance-none bg-gray-100 border-2 border-gray-200 rounded-md w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-blue-500 focus:shadow-outline-blue"
            id="email"
            type="email"
            v-model="personalInfo.email"
            required
          />
        </div>
        <div>
          <label class="block text-gray-700 font-bold mb-1 text-lg" for="phone">
            Phone Number <span class="text-red-600">*</span>
          </label>
          <input
            class="appearance-none bg-gray-100 border-2 border-gray-200 rounded-md w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-blue-500 focus:shadow-outline-blue"
            id="phone"
            type="tel"
            v-model="personalInfo.phone"
            required
          />
        </div>
        <div>
          <label class="block text-gray-700 font-bold mb-1 text-lg" for="address"> Address </label>
          <input
            class="appearance-none bg-gray-100 border-2 border-gray-200 rounded-md w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-blue-500 focus:shadow-outline-blue"
            id="address"
            type="text"
            v-model="personalInfo.address"
          />
        </div>
      </div>
    </form>
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue'
interface PersonalInfo {
  name: string
  email: string
  phone: string
  address: string
}

const personalInfo = ref<PersonalInfo>({
  name: '',
  email: '',
  phone: '',
  address: ''
})

function submitForm() {
  // Code to save the form data
}
</script>
