<template>
    <div class="bg-white p-8">
        <h2 class="text-xl font-bold mb-4">Pipeline Management</h2>
        <div class="flex justify-center">
            <!-- Add components for each stage of the pipeline -->
            <Stage stage="sourcing" :candidates="sourcingCandidates" />
            <Stage stage="screening" :candidates="screeningCandidates" />
            <Stage stage="interviewing" :candidates="interviewingCandidates" />
            <Stage stage="assessing" :candidates="hiringCandidates" />
        </div>
        <div class="mt-8 flex justify-between items-center">
            <div class="w-1/2">
                <input class="border rounded-lg w-full py-2 px-3" type="text" placeholder="Search candidates"
                    v-model="searchTerm">
            </div>
            <div class="flex items-center">
                <p class="mr-4">Sort by:</p>
                <select class="border rounded-lg py-2 px-3" v-model="sortBy">
                    <option value="name">Name</option>
                    <option value="dateAdded">Date Added</option>
                    <option value="stage">Stage</option>
                </select>
            </div>

        </div>
        <div class="mt-8">
            <h3 class="text-lg font-bold mb-4">Manage Employee Stage</h3>
            <div class="mt-8 py-2 px-4 rounded-lg shadow  overflow-y-auto ">
                <ul class="max-h-[300px]">
                    <li v-for="candidate in filteredCandidates" :key="candidate.id"
                        class="py-4 border-b border-gray-200 flex justify-between items-center">
                        <div>
                            <p class="font-medium">{{ candidate.name }}</p>
                            <p class="text-gray-500">{{ candidate.stage }}</p>
                        </div>
                        <div class="flex items-center">
                            <select class="border rounded-lg py-2 px-3" v-model="candidate.stage">
                                <option value="sourcing">Sourcing</option>
                                <option value="screening">Screening</option>
                                <option value="interviewing">Interviewing</option>
                                <option value="hiring">Assessing</option>

                            </select>

                        </div>
                    </li>
                </ul>
            </div>
        </div>
        <div class="mt-8">
            <h3 class="text-lg font-bold mb-4">Add candidate</h3>
            <div class="py-2 px-4 rounded-lg shadow  overflow-y-auto ">
                <form @submit.prevent="handleSubmit">
                    <div class="flex flex-col mb-4">
                        <label class="mb-2 font-medium" for="name">Name</label>
                        <input class="border rounded-lg py-2 px-3" type="text" id="name" v-model="name" required>
                    </div>
                    <div class="flex flex-col mb-4">
                        <label class="mb-2 font-medium" for="email">Email</label>
                        <input class="border rounded-lg py-2 px-3" type="email" id="email" v-model="email" required>
                    </div>
                    <div class="flex flex-col mb-4">
                        <label class="mb-2 font-medium" for="email">Stage</label>

                        <select class="border rounded-lg py-2 px-3" v-model="stage">
                            <option value="sourcing">Sourcing</option>
                            <option value="screening">Screening</option>
                            <option value="interviewing">Interviewing</option>
                            <option value="hiring">Assessing</option>

                        </select>
                    </div>
                    <div class="flex justify-end">
                        <button class="bg-blue-500 text-white px-4 py-2 rounded-lg" type="submit">Add Candidate</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>
  
<script setup lang="ts">
import { ref, computed } from 'vue'
import Stage from './Stage.vue'
import type { Candidate } from '../../types/Candidate'
import { v4 as uuidv4 } from 'uuid'
const emit = defineEmits(['add-candidate'])

const name = ref('')
const email = ref('')
const stage = ref('')

function handleSubmit() {
    if (candidates.value.find((c) => c.email === email.value)?.id) {
        alert("Candidate already exists!")
        return;
    }

    candidates.value.push({
        id: uuidv4(),
        name: name.value,
        email: email.value,
        stage: stage.value,
        dateAdded: new Date().toISOString(),
    });

    name.value = ''
    email.value = ''
    stage.value = ''
}
const candidates = ref<Candidate[]>([
    {
        id: '1',
        name: 'John Doe',
        email: 'johndoe@example.com',
        stage: 'sourcing',
        dateAdded: '2022-02-01',
    },
    {
        id: '2',
        name: 'Jane Smith',
        email: 'janesmith@example.com',
        stage: 'screening',
        dateAdded: '2022-02-01',
    },
    {
        id: '3',
        name: 'Bob Johnson',
        email: 'bobjohnson@example.com',
        stage: 'interviewing',
        dateAdded: '2022-02-01',
    },
    {
        id: '4',
        name: 'Sarah Lee',
        email: 'sarahlee@example.com',
        stage: 'hiring',
        dateAdded: '2022-02-01',
    },
])

const sourcingCandidates = computed(() => {
    return candidates.value.filter(candidate => candidate.stage === 'sourcing')
})

const screeningCandidates = computed(() => {
    return candidates.value.filter(candidate => candidate.stage === 'screening')
})

const interviewingCandidates = computed(() => {
    return candidates.value.filter(candidate => candidate.stage === 'interviewing')
})

const hiringCandidates = computed(() => {
    return candidates.value.filter(candidate => candidate.stage === 'hiring')
})

const searchTerm = ref('')
const sortBy = ref('name')

const filteredCandidates = computed(() => {
    let sortedCandidates = candidates.value.slice()
    if (searchTerm.value !== '') {
        sortedCandidates = sortedCandidates.filter(candidate =>
            candidate.name.toLowerCase().includes(searchTerm.value.toLowerCase())
        )
    }
    sortedCandidates.sort((a, b) => {
        if (sortBy.value === 'dateAdded') {
            return new Date(b.dateAdded).getTime() - new Date(a.dateAdded).getTime()
        } else if (sortBy.value === 'stage') {
            return b.stage.localeCompare(a.stage)
        } else {
            return a.name.localeCompare(b.name)
        }
    })
    return sortedCandidates
});

function changeCandidateStage(candidate: Candidate, newStage: string) {
    candidate.stage = newStage
}

function addCandidate(candidate: Candidate) {
    candidates.value.push(candidate)
}
</script>