<template>
    <div class="bg-white p-8">
        <h2 class="text-xl font-bold mb-4">Team Performance</h2>
        <div class="flex justify-between">
            <div class="w-1/2 pr-4">
                <div class="bg-gray-100 rounded-lg p-4 mb-4">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-lg font-medium mb-2">Employees screened</p>
                            <p class="text-gray-500">{{ candidatesScreened }} screened this month</p>
                        </div>
                        <p class="text-4xl font-bold">{{ candidatesScreened }}</p>
                    </div>
                </div>
                <div class="bg-gray-100 rounded-lg p-4 mb-4">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-lg font-medium mb-2">Assessments made</p>
                            <p class="text-gray-500">{{ hiresMade }} made this month</p>
                        </div>
                        <p class="text-4xl font-bold">{{ hiresMade }}</p>
                    </div>
                </div>
            </div>
            <div class="w-1/2 pl-4">
                <div class="bg-gray-100 rounded-lg p-4 mb-4">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-lg font-medium mb-2">Interviews conducted</p>
                            <p class="text-gray-500">{{ interviewsConducted }} conducted this month</p>
                        </div>
                        <p class="text-4xl font-bold">{{ interviewsConducted }}</p>
                    </div>
                </div>
                <div class="bg-gray-100 rounded-lg p-4 mb-4">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-lg font-medium mb-2">Offers extended</p>
                            <p class="text-gray-500">{{ offersExtended }} extended this month</p>
                        </div>
                        <p class="text-4xl font-bold">{{ offersExtended }}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
  
<script setup lang="ts">
defineProps({
    candidatesScreened: {
        type: Number,
        required: true,
        default: 0,
    },
    hiresMade: {
        type: Number,
        required: true,
        default: 0,
    },
    interviewsConducted: {
        type: Number,
        required: true,
        default: 0,
    },
    offersExtended: {
        type: Number,
        required: true,
        default: 0,
    },
})
</script>
  
<style scoped></style>
  