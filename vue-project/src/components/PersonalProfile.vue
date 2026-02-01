<script setup>
import { ref, onMounted } from 'vue'
import { supabase } from '../lib/supabaseClient'

const sports = ref([])

async function getSports() {
  const { data, error } = await supabase
    .from('sports')
    .select('Name')
  
  if (data) {
    sports.value = data
  }
}

onMounted(() => {
  getSports()
})
</script>

<template>
  <div class="table-only-view">
    <ul>
      <li v-for="sport in sports" :key="sport.id">
        {{ sport.Name }}
      </li>
    </ul>
  </div>
</template>

<style scoped>
.table-only-view {
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
  line-height: 1.5;
}
</style>