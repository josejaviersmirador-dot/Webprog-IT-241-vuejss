<script setup>
import { ref, onMounted } from 'vue' 
import { supabase } from '../lib/supabaseClient'

const comments = ref([])
const userName = ref('') // Added for the Name input
const newComment = ref('')

const fetchComments = async () => {
  const { data, error } = await supabase
    .from('comments')
    .select('*')
    .order('created_at', { ascending: false })
  
  if (error) console.error(error)
  else comments.value = data
}

const postComment = async () => {
  if (!newComment.value.trim() || !userName.value.trim()) {
    alert("Please fill in both Name and Comment!")
    return
  }

  const { error } = await supabase.from('comments').insert([
    { 
      user_name: userName.value, // Make sure your DB column is named 'user_name'
      content: newComment.value, 
      page_path: window.location.pathname 
    }
  ])

  if (error) {
    alert("Error: " + error.message)
  } else {
    newComment.value = ''
    userName.value = ''
    fetchComments()
  }
}

onMounted(() => {
  fetchComments()
})
</script>

<template>
  <div class="feedback-container">
    <div class="comment-form">
      <h2>Leave a Comment</h2>
      <label>Name:</label>
      <input v-model="userName" type="text" placeholder="Your Name" />
      
      <label>Comment:</label>
      <textarea v-model="newComment" placeholder="Your message..."></textarea>
      
      <button @click="postComment">Submit</button>
    </div>

    <div class="comments-list">
      <h2>Comments</h2>
      <ul>
        <li v-for="c in comments" :key="c.id">
          <strong>{{ c.user_name }}</strong>: {{ c.content }}
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.feedback-container {
  max-width: 500px;
  margin: 20px;
  font-family: Arial, sans-serif;
}

.comment-form {
  border: 1px dashed black;
  padding: 15px;
  background-color: #fdfde0; /* Light yellow like your image */
}

input, textarea {
  display: block;
  width: 100%;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  padding: 5px;
}

button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 8px 15px;
  border-radius: 4px;
  cursor: pointer;
}

ul {
  list-style-type: disc;
  padding-left: 20px;
}

li {
  margin-bottom: 5px;
}
</style>