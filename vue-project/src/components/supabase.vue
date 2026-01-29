<script setup>
import { ref, onMounted } from 'vue'
import { supabase } from '../lib/supabaseClient'

const projects = ref([])

async function getProjects() {
  let { data, error } = await supabase
    .from('projects')
    .select('*')

  if (data) {
    projects.value = data
  }
}

onMounted(() => {
  getProjects()
})
</script>