<script setup>
import { ref, onMounted } from 'vue'
import { supabase } from '../lib/supabaseClient'

const sportsList = ref([])

async function fetchSports() {
  // Uses 'Sport' (singular) to match your Supabase table name exactly
  const { data, error } = await supabase
    .from('Sport') 
    .select('Name')
  
  if (error) {
    console.error('Error fetching data:', error.message)
  } else {
    sportsList.value = data
  }
}

onMounted(() => {
  fetchSports()
})
</script>

<template>
  <div class="list-container">
    <ul v-if="sportsList.length > 0">
      <li v-for="sport in sportsList" :key="sport.id">
        {{ sport.Name }}
      </li>
    </ul>
    <p v-else>Connecting to database...</p>
  </div>
</template>

<style scoped>
.list-container {
  padding: 20px;
}
ul {
  list-style-type: disc;
  padding-left: 40px;
}
li {
  font-family: serif;
  font-size: 1.2rem;
  margin-bottom: 5px;
  color: black;
}
</style>