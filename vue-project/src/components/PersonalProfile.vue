<script setup>
import { ref, onMounted } from 'vue'
import { supabase } from '../lib/supabaseClient'

const sportsList = ref([])
const errorDetails = ref('')

async function fetchSports() {
  try {
    const { data, error } = await supabase
      .from('Sport') 
      .select('Name')
    
    if (error) {
      errorDetails.value = error.message
      console.error('Fetch Error:', error)
    } else if (data) {
      sportsList.value = data
    }
  } catch (err) {
    errorDetails.value = "Check your Vercel Environment Variables."
    console.error('Connection Error:', err)
  }
}

onMounted(() => {
  fetchSports()
})
</script>

<template>
  <div class="list-container">
    <p v-if="errorDetails" style="color: red;">Error: {{ errorDetails }}</p>
    
    <ul v-else-if="sportsList.length > 0">
      <li v-for="sport in sportsList" :key="sport.Name">
        {{ sport.Name }}
      </li>
    </ul>
    
    <ul v-else>
      <li>Basketball</li>
      <li>Badminton</li>
      <li>Volleyball</li>
    </ul>
  </div>
</template>

<style scoped>
.list-container {
  padding: 20px;
  background: white;
}
ul {
  list-style-type: disc;
  padding-left: 40px;
  margin: 0;
}
li {
  color: black;
  font-family: serif;
  font-size: 18px;
  line-height: 1.6;
}
</style>