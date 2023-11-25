<template>
  <div class="bg-white p-6 rounded-md shadow-lg">
    <h2 class="text-3xl font-bold text-gray-800 mb-4">Education</h2>
    <form @submit.prevent="submitForm" class="space-y-6">
      <div v-for="(item, index) in education" :key="index">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div>
            <label class="block text-gray-700 font-bold mb-1 text-lg" :for="'degree-' + index">
              Degree <span class="text-red-600">*</span>
            </label>
            <input
              class="appearance-none bg-gray-100 border-2 border-gray-200 rounded-md w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-blue-500 focus:shadow-outline-blue"
              :id="'degree-' + index"
              type="text"
              v-model="item.degree"
              required
            />
          </div>
          <div>
            <label class="block text-gray-700 font-bold mb-1 text-lg" :for="'institution-' + index">
              Institution <span class="text-red-600">*</span>
            </label>
            <input
              class="appearance-none bg-gray-100 border-2 border-gray-200 rounded-md w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-blue-500 focus:shadow-outline-blue"
              :id="'institution-' + index"
              type="text"
              v-model="item.institution"
              required
            />
          </div>
          <div>
            <label class="block text-gray-700 font-bold mb-1 text-lg" :for="'startYear-' + index">
              Start Year <span class="text-red-600">*</span>
            </label>
            <input
              class="appearance-none bg-gray-100 border-2 border-gray-200 rounded-md w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-blue-500 focus:shadow-outline-blue"
              :id="'start-year-' + index"
              type="date"
              v-model="item.startYear"
              required
            />
          </div>
          <div>
            <label class="block text-gray-700 font-bold mb-1 text-lg" :for="'endYear-' + index">
              End Year
            </label>
            <input
              class="appearance-none bg-gray-100 border-2 border-gray-200 rounded-md w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-blue-500 focus:shadow-outline-blue"
              :id="'end-year-' + index"
              type="date"
              v-model="item.endYear"
              required
            />
          </div>
        </div>
        <div class="flex justify-end">
          <button
            type="button"
            class="block mt-2 bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline-red"
            @click="deleteItem(index)"
          >
            Remove
          </button>
        </div>
      </div>
      <div class="flex justify-end">
        <button
          type="button"
          class="bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline-green"
          @click="addItem"
        >
          Add Education
        </button>
      </div>
    </form>
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue'
interface Education {
  degree: string
  institution: string
  startYear: number
  endYear?: number
}
const education = ref<Education[]>([
  {
    degree: '',
    institution: '',
    startYear: 0,
    endYear: 0
  }
])

function addItem() {
  education.value.push({
    degree: '',
    institution: '',
    startYear: 0,
    endYear: 0
  })
}

function deleteItem(index: number) {
  education.value.splice(index, 1)
}

function submitForm() {
  // Code to save the form data
}
</script>
