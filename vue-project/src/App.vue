<script setup>
import RestApi from './components/RestApi.vue' // Add this line
import PersonalProfile from './components/PersonalProfile.vue'
import Education from './components/Education.vue'
</script>

<template>
  <div class="portfolio-wrapper">
    <h1>My Portfolio</h1>
    
    <RestApi /> <PersonalProfile />
    <Education />
  </div>
</template>