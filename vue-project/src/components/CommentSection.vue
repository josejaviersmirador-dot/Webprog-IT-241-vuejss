<script setup>
import { ref, onMounted } from 'vue'
import { supabase } from '../lib/supabaseClient' 

const comments = ref([])
const newComment = ref('')

const fetchComments = async () => {
  const { data } = await supabase
    .from('comments')
    .select('*')
    .order('created_at', { ascending: false })
  comments.value = data
}

const postComment = async () => {
  if (!newComment.value.trim()) {
    alert("Please write something before posting!");
    return;
  }

  const { error } = await supabase.from('comments').insert([
    { content: newComment.value, page_path: window.location.pathname }
  ])
  
  if (error) {
    alert("Error: " + error.message);
  } else {
    newComment.value = ''
    fetchComments()
  }
}

onMounted(() => fetchComments())
</script>

<template>
  <div class="comment-section">
    <textarea v-model="newComment" placeholder="Leave a comment..."></textarea>
    <button @click="postComment">Post Comment</button>

    <div v-for="c in comments" :key="c.id" style="margin-top: 10px; border-bottom: 1px solid #ccc;">
      <p>{{ c.content }}</p>
      <small>{{ new Date(c.created_at).toLocaleString() }}</small>
    </div>
  </div>
</template>
<style scoped>
.comment-section {
  max-width: 600px;
  margin: 20px auto;
  font-family: sans-serif;
}

textarea {
  width: 100%;
  height: 80px;
  margin-bottom: 10px;
  padding: 10px;
}

.comment-card {
  background: #f9f9f9;
  border-left: 4px solid #3ecf8e; /* Supabase Green */
  padding: 10px;
  margin: 10px 0;
  border-radius: 4px;
}

small {
  color: #666;
}
</style>