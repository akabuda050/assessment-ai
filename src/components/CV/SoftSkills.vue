<template>
  <div class="bg-white p-6 rounded-md shadow-lg">
    <h2 class="text-3xl font-bold text-gray-800 mb-4">Soft Skills</h2>
    <form @submit.prevent="submitForm" class="space-y-6">
      <div v-for="(skill, index) in softSkills" :key="index">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div>
            <label class="block text-gray-700 font-bold mb-1 text-lg" :for="'skill-' + index">
              Skill <span class="text-red-600">*</span>
            </label>
            <input
              class="appearance-none bg-gray-100 border-2 border-gray-200 rounded-md w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-blue-500 focus:shadow-outline-blue"
              :id="'skill-' + index"
              type="text"
              v-model="skill.skill"
              required
            />
          </div>
          <div>
            <label class="block text-gray-700 font-bold mb-1 text-lg" :for="'proficiency-' + index">
              Proficiency <span class="text-red-600">*</span>
            </label>
            <select
              class="appearance-none bg-gray-100 border-2 border-gray-200 rounded-md w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-blue-500 focus:shadow-outline-blue"
              :id="'proficiency-' + index"
              v-model="skill.proficiency"
              required
            >
              <option value="">Select Proficiency</option>
              <option value="Beginner">Beginner</option>
              <option value="Intermediate">Intermediate</option>
              <option value="Advanced">Advanced</option>
            </select>
          </div>
        </div>
        <div class="flex justify-end">
          <button
            type="button"
            class="block mt-2 bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline-red"
            @click="removeSkill(index)"
          >
            Remove
          </button>
        </div>
      </div>
      <div class="flex justify-end">
        <button
          type="button"
          class="bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline-green"
          @click="addSkill"
        >
          Add Soft Skill
        </button>
      </div>
    </form>
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue'
interface SoftSkill {
  skill: string
  proficiency: string
}
const softSkills = ref<SoftSkill[]>([{ skill: '', proficiency: '' }])

function addSkill() {
  softSkills.value.push({ skill: '', proficiency: '' })
}

function removeSkill(index: number) {
  softSkills.value.splice(index, 1)
}

function submitForm() {
  // Code to save the form data
}
</script>
