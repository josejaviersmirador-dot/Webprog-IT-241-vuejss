<script setup>
import { ref, onMounted } from 'vue'
import { supabase } from '../lib/supabaseClient'

const sports = ref([])

async function getSports() {
  // Matches the table name 'Sport' from your screenshot
  let { data, error } = await supabase
    .from('Sport')
    .select('*')

  if (data) {
    sports.value = data
  } else {
    console.error('Error loading sports:', error)
  }
}

onMounted(() => {
  getSports()
})
</script>

<template>
  <div class="interests-container">
    <h2>Interests</h2>
    <ul>
      <li v-for="sport in sports" :key="sport.id">
        {{ sport.Name }}
      </li>
    </ul>
  </div>
</template>

<style scoped>
.interests-container {
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  margin-top: 20px;
  color: #333;
}
ul {
  list-style-type: square;
  padding-left: 20px;
}
</style>