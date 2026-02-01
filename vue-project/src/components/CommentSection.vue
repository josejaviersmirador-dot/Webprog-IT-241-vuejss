<script setup>
import { ref, onMounted, computed } from 'vue' 
import { supabase } from '../lib/supabaseClient'

const comments = ref([])
const newComment = ref('')

const charCount = computed(() => newComment.value.length)
const maxChars = 280

// TASK: The Delete Function
const deleteComment = async (id) => {
  const { error } = await supabase
    .from('comments')
    .delete()
    .eq('id', id)

  if (error) {
    alert("Error deleting: " + error.message)
  } else {
    fetchComments()
  }
}

const postComment = async () => {
  if (!newComment.value.trim()) {
    alert("Please enter a message!")
    return
  }
}

<template>
  <div class="comment-section">
    <textarea v-model="newComment" :maxlength="maxChars" placeholder="Leave a comment..."></textarea>
    
    <p :class="{ 'text-danger': charCount >= maxChars }">
      {{ charCount }} / {{ maxChars }} characters
    </p>

    <button @click="postComment">Post Comment</button>

    <div v-for="c in comments" :key="c.id" class="comment-card">
      <p>{{ c.content }}</p>
      <small>{{ new Date(c.created_at).toLocaleString() }}</small>
      
      <button @click="deleteComment(c.id)" class="delete-btn">Delete</button>
    </div>
  </div>
</template>

<style scoped>
p {
  font-size: 0.8rem;
  color: #666;
  margin-top: -5px;
}

.text-danger {
  color: red;
  font-weight: bold;
}

/* Styling the Delete Button */
.delete-btn {
  background: none;
  border: none;
  color: #ff4d4d;
  cursor: pointer;
  font-size: 0.8rem;
  margin-left: 10px;
  text-decoration: underline;
}

.delete-btn:hover {
  color: #cc0000;
}
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
  border-left: 4px solid #3ecf8e;
  padding: 10px;
  margin: 10px 0;
  border-radius: 4px;
}

small {
  color: #666;
}
</style>