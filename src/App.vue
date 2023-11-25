<template>
  <div class="max-w-3xl mx-auto flex flex-col gap-2">
    <Personalinformation />
    <WorkExperience />
    <TechnicalSkills />
    <SoftSkills />
    <TransferableSkills />
    <LanguageProficiency />
    <Education />
  </div>
</template>
<script setup lang="ts">
import Education from './components/CV/Education.vue'
import LanguageProficiency from './components/CV/LanguageProficiency.vue'
import Personalinformation from './components/CV/Personalinformation.vue'
import SoftSkills from './components/CV/SoftSkills.vue'
import TechnicalSkills from './components/CV/TechnicalSkills.vue'
import TransferableSkills from './components/CV/TransferableSkills.vue'
import WorkExperience from './components/CV/WorkExperience.vue'
</script>
