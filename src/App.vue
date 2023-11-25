<template>
  <div>
    <WelcomeMessage />
    <KeyMetrics :candidates="17" :interviews="5" time-to-hire="10 days"/>
    <CandidatePipelineOverview :sourcing="10" :screening="5" :interviewing="2" :hiring="1" />
    <CandidateProfiles />
    <TeamPerformance :candidates-screened="5" :hires-made="10" :interviews-conducted="5" :offers-extended="5"/>
    <Notifications />
    <PipelineManagement />
  </div>
</template>
<script setup lang="ts">
import WelcomeMessage from './components/Dashboard/WelcomeMessage.vue';
import KeyMetrics from './components/Dashboard/KeyMetrics.vue';
import CandidatePipelineOverview from './components/Dashboard/CandidatePipelineOverview.vue';
import CandidateProfiles from './components/Dashboard/CandidateProfiles.vue';
import TeamPerformance from './components/Dashboard/TeamPerformance.vue';
import Notifications from './components/Dashboard/Notifications.vue';
import PipelineManagement from './components/Dashboard/PipelineManagement.vue';

</script>