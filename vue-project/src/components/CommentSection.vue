<script setup>
import { ref, onMounted, computed } from 'vue' 
import { supabase } from '../lib/supabaseClient'

const comments = ref([])
const newComment = ref('')

const charCount = computed(() => newComment.value.length)
const maxChars = 280

// Fetch comments from Supabase
const fetchComments = async () => {
  const { data, error } = await supabase
    .from('comments')
    .select('*')
    .order('created_at', { ascending: false })
  
  if (error) console.error(error)
  else comments.value = data
}

// Post a new comment
const postComment = async () => {
  if (!newComment.value.trim()) {
    alert("Please enter a message!")
    return
  }

  const { error } = await supabase.from('comments').insert([
    { content: newComment.value, page_path: window.location.pathname }
  ])

  if (error) {
    alert("Error: " + error.message)
  } else {
    newComment.value = ''
    fetchComments()
  }
}

// Delete a comment
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

onMounted(() => {
  fetchComments()
})
</script> <template>
  <div class="comment-section">
    <textarea v-model="newComment" :maxlength="maxChars" placeholder="Leave a comment..."></textarea>
    
    <p :class="{ 'text-danger': charCount >= maxChars }">
      {{ charCount }} / {{ maxChars }} characters
    </p>

    <button @click="postComment">Post Comment</button>

    <div v-if="comments.length === 0" style="margin-top: 20px; color: gray;">
      No comments yet.
    </div>

    <div v-for="c in comments" :key="c.id" class="comment-card">
      <p>{{ c.content }}</p>
      <small>{{ new Date(c.created_at).toLocaleString() }}</small>
      <button @click="deleteComment(c.id)" class="delete-btn">Delete</button>
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

p {
  font-size: 0.8rem;
  color: #666;
  margin-top: -5px;
}

.text-danger {
  color: red;
  font-weight: bold;
}

.comment-card {
  background: #f9f9f9;
  border-left: 4px solid #3ecf8e;
  padding: 10px;
  margin: 10px 0;
  border-radius: 4px;
  position: relative;
}

small {
  color: #666;
  display: block;
}

.delete-btn {
  background: none;
  border: none;
  color: #ff4d4d;
  cursor: pointer;
  font-size: 0.75rem;
  margin-top: 5px;
  padding: 0;
  text-decoration: underline;
}

.delete-btn:hover {
  color: #cc0000;
}
</style>