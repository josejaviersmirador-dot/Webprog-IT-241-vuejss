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
  alert("Button clicked!"); // Add this line
  const { error } = await supabase.from('comments').insert([
    { content: newComment.value, page_path: window.location.pathname }
  ])
  
  if (error) {
    alert("Error: " + error.message);
  } else {
    alert("Success!");
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