<template>
  <div class="bg-white p-6 rounded-md shadow-lg">
    <h2 class="text-3xl font-bold text-gray-800 mb-4">Work Experience</h2>
    <form @submit.prevent="submitForm" class="space-y-6">
      <div v-for="(experience, index) in workExperience" :key="index">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div>
            <label class="block text-gray-700 font-bold mb-1 text-lg" :for="'job-title-' + index">
              Job Title <span class="text-red-600">*</span>
            </label>
            <input
              class="appearance-none bg-gray-100 border-2 border-gray-200 rounded-md w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-blue-500 focus:shadow-outline-blue"
              :id="'job-title-' + index"
              type="text"
              v-model="experience.jobTitle"
              required
            />
          </div>
          <div>
            <label class="block text-gray-700 font-bold mb-1 text-lg" :for="'employer-' + index">
              Employer <span class="text-red-600">*</span>
            </label>
            <input
              class="appearance-none bg-gray-100 border-2 border-gray-200 rounded-md w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-blue-500 focus:shadow-outline-blue"
              :id="'employer-' + index"
              type="text"
              v-model="experience.employer"
              required
            />
          </div>
          <div>
            <label class="block text-gray-700 font-bold mb-1 text-lg" :for="'start-date-' + index">
              Start Date <span class="text-red-600">*</span>
            </label>
            <input
              class="appearance-none bg-gray-100 border-2 border-gray-200 rounded-md w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-blue-500 focus:shadow-outline-blue"
              :id="'start-date-' + index"
              type="date"
              v-model="experience.startDate"
              required
            />
          </div>
          <div>
            <label class="block text-gray-700 font-bold mb-1 text-lg" :for="'end-date-' + index">
              End Date
            </label>
            <input
              class="appearance-none bg-gray-100 border-2 border-gray-200 rounded-md w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-blue-500 focus:shadow-outline-blue"
              :id="'end-date-' + index"
              type="date"
              v-model="experience.endDate"
            />
          </div>
          <div>
            <label class="block text-gray-700 font-bold mb-1 text-lg" :for="'description-' + index">
              Description
            </label>
            <textarea
              class="appearance-none bg-gray-100 border-2 border-gray-200 rounded-md w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-blue-500 focus:shadow-outline-blue"
              :id="'description-' + index"
              v-model="experience.description"
              rows="3"
            ></textarea>
          </div>
        </div>
        <div class="flex justify-end">
          <button
            type="button"
            class="block mt-2 bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline-red"
            @click="deleteExperience(index)"
          >
            Remove
          </button>
        </div>
      </div>
      <div class="flex justify-end">
        <button
          type="button"
          class="bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline-green"
          @click="addExperience"
        >
          Add Experience
        </button>
      </div>
    </form>
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue'

interface WorkExperience {
  jobTitle: string
  employer: string
  startDate: string
  endDate?: string
  description?: string
}

const workExperience = ref<WorkExperience[]>([
  {
    jobTitle: '',
    employer: '',
    startDate: ''
  }
])

function addExperience() {
  workExperience.value.push({
    jobTitle: '',
    employer: '',
    startDate: ''
  })
}

function deleteExperience(index: number) {
  workExperience.value.splice(index, 1)
}

function submitForm() {
  // Code to save the form data
}
</script>
