<script setup>
import { ref, onMounted } from 'vue'
import { supabase } from '../lib/supabaseClient'

const sportsList = ref([])
const errorMessage = ref('')

async function fetchSports() {
  try {
    const { data, error } = await supabase
      .from('Sport') // Must match your table name exactly
      .select('Name')
    
    if (error) {
      errorMessage.value = error.message
      console.error('Database error:', error)
    } else {
      sportsList.value = data
    }
  } catch (err) {
    errorMessage.value = "Failed to connect to Supabase."
    console.error('Connection error:', err)
  }
}

onMounted(() => {
  fetchSports()
})
</script>

<template>
  <div class="list-container">
    <p v-if="errorMessage" style="color: red;">Error: {{ errorMessage }}</p>
    
    <ul v-else-if="sportsList.length > 0">
      <li v-for="sport in sportsList" :key="sport.id">
        {{ sport.Name }}
      </li>
    </ul>
    
    <p v-else>Connecting to database...</p>
  </div>
</template>

<style scoped>
.list-container { padding: 20px; }
ul { list-style-type: disc; padding-left: 40px; }
li { font-family: serif; font-size: 1.2rem; color: black; }
</style>