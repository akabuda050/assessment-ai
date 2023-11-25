<template>
  <div class="max-w-5xl mx-auto">
    <form class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4" enctype="multipart/form-data"
      @submit.prevent="uploadFile">
      <div class="mb-4">
        <label class="block text-gray-700 font-bold mb-2" for="file"> Upload CV </label>
        <input ref="fileInput"
          class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
          id="file" type="file" name="file" />
      </div>
      <div class="flex items-center justify-between">
        <button
          class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
          type="submit">
          Upload
        </button>
      </div>
    </form>

    <div class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4">
      <ul v-if="analysis?.result" class="mb-4">
        <li class="mb-2">
          <strong>Name:</strong><br />
          <p class="text-sm">{{ analysis.result.name }}</p>
        </li>
        <li class="mb-2">
          <strong>Email:</strong><br />
          <p class="text-sm">{{ analysis.result.email }}</p>
        </li>
        <li class="mb-2">
          <strong>Area Of Expertise:</strong><br />
          <p class="text-sm">{{ analysis.result.areaOfExpertise }}</p>
        </li>
        <li class="mb-2">
          <strong>Technologies:</strong><br />
          <ul class="list-disc pl-6 text-sm">
            <li v-for="technology in  analysis.result.technologies" :key="technology">
              <p class="text-sm">{{ technology }}</p>
            </li>
          </ul>
        </li>
        <li class="mb-2">
          <strong>Skills:</strong><br />
          <ul class="list-disc pl-6 text-sm">
            <li v-for="skill in  analysis.result.skills" :key="skill">
              <p class="text-sm">{{ skill }}</p>
            </li>
          </ul>
        </li>
        <li class="mb-2">
          <strong>Key Responsibilities:</strong><br />
          <ul class="list-disc pl-6 text-sm">
            <li v-for="keyResponsibility in  analysis.result.keyResponsibilities" :key="keyResponsibility">
              <p class="text-sm">{{ keyResponsibility }}</p>
            </li>
          </ul>
        </li>
        <li class="mb-2">
          <strong>Possible experience in years:</strong><br />
          <p class="text-sm">{{ analysis.result.experience }}</p>
        </li>
        <li class="mb-2">
          <strong>Possible grade:</strong><br />
          <p class="text-sm">{{ analysis.result.grade }}</p>
        </li>
      </ul>

      <p v-if="analysis?.summary" class="text-sm">
        <strong>Summary:</strong><br /> {{ analysis?.summary.summary }}
      </p>
      <p v-if="!analysis && !loading" class="text-center mt-4 text-sm font-semibold">
        No Info. Please upload CV
      </p>
      <p v-if="!analysis && loading" class="text-center my-4 text-sm font-semibold">
        CV is being processed. Please wait.
      </p>
      <transition name="fade">
        <div v-if="!analysis && loading" class="typing-indicator flex justify-center pl-1">
          <span class="h-2 w-2 rounded-full bg-gray-400 mr-1"></span>
          <span class="h-2 w-2 rounded-full bg-gray-400 mr-1"></span>
          <span class="h-2 w-2 rounded-full bg-gray-400"></span>
        </div>
      </transition>
    </div>

    <div class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4">
      <ul v-if="questions.length" class="mb-4">
        <li v-for="question in questions" class="mb-2">
          <strong>Question:</strong><br />
          <p class="text-sm">{{ question.details || '' }}</p>

          <strong>Time Limit:</strong><br />
          <p class="text-sm">{{ question.timeLimit }}</p>
        </li>
      </ul>
      <p v-if="!questions.length && !loading" class="text-center mt-4 text-sm font-semibold">
        No Questions. Please upload CV
      </p>
      <p v-if="!questions.length && loading" class="text-center my-4 text-sm font-semibold">
        We are prepairing questions. Please wait.
      </p>
      <transition name="fade">
        <div v-if="!questions.length && loading" class="typing-indicator flex justify-center pl-1">
          <span class="h-2 w-2 rounded-full bg-gray-400 mr-1"></span>
          <span class="h-2 w-2 rounded-full bg-gray-400 mr-1"></span>
          <span class="h-2 w-2 rounded-full bg-gray-400"></span>
        </div>
      </transition>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { ref } from 'vue'
import axios from 'axios'
const fileInput = ref<HTMLInputElement | null>(null)
const loading = ref(false);

const analysis = ref();
const questions = ref<{ details: string, timeLimit: number }[]>([]);

const uploadFile = async () => {
  console.log(fileInput.value)
  const file = fileInput.value?.files?.[0]
  if (!file) {
    console.log('Please select a file to upload.')
    return
  }

  const formData = new FormData()
  formData.append('file', file)

  loading.value = true;

  try {
    analysis.value = (await axios.post('http://localhost:3001/assessment/analyze-cv', formData)).data
    questions.value = (await axios.post('http://localhost:3001/assessment/get-questions', {
      candidateInformation: analysis.value.result
    })).data?.questions || [];


  } catch (error) {
    console.log(error)
  } finally {
    loading.value = false;
  }
}
</script>
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