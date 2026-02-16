<script setup>
import RestApi from './components/RestApi.vue'
import PersonalProfile from './components/PersonalProfile.vue'
import Education from './components/Education.vue'
</script>

<template>
  <div class="portfolio-wrapper">
    <h1>My Portfolio</h1>
    
    <RestApi />

    <hr />
    
    <PersonalProfile />
    <Education />
  </div>
</template>

<style scoped>
hr {
  margin: 40px 0;
  border: 1px solid #444;
}
</style>