<template>
  <div class="api-container">
    <h2>JSONPlaceholder API Demo</h2>

    <div class="add-post-form">
      <input v-model="newPost.title" placeholder="Enter title" />
      <textarea v-model="newPost.body" placeholder="Enter body"></textarea>
      <button @click="addPost">Add Post</button>
    </div>

    <h3>Posts</h3>
    <div v-if="loading">Loading data from API...</div>
    <div v-if="error" class="error-msg">{{ error }}</div>

    <ul v-else>
      <li v-for="post in posts" :key="post.id">
        <span class="post-text">{{ post.title }} (ID: {{ post.id }})</span>
        <div class="actions">
          <button class="edit-btn">Edit</button>
          <button @click="deletePost(post.id)" class="delete-btn">Delete</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

const apiClient = axios.create({
  baseURL: import.meta.env.VITE_API_URL || 'https://jsonplaceholder.typicode.com',
  headers: { 'Content-Type': 'application/json' }
});

export default {
  data() {
    return {
      posts: [],
      loading: true,
      error: null,
      newPost: { title: '', body: '' }
    };
  },
  methods: {
    async fetchPosts() {
      try {
        const response = await apiClient.get('/posts');
        this.posts = response.data;
      } catch (err) {
        this.error = "Failed to load posts: " + err.message;
      } finally {
        this.loading = false;
      }
    },
    async addPost() {
      try {
        const response = await apiClient.post('/posts', this.newPost);
        this.posts.unshift(response.data);
        this.newPost = { title: '', body: '' };
      } catch (err) {
        this.error = "Add failed: " + err.message;
      }
    },
    async deletePost(id) {
      try {
        await apiClient.delete(`/posts/${id}`);
        this.posts = this.posts.filter(post => post.id !== id);
      } catch (err) {
        this.error = "Delete failed: " + err.message;
      }
    }
  },
  mounted() {
    this.fetchPosts();
  }
};
</script>

<style scoped>
.api-container {
  background-color: #fffde7;
  padding: 20px;
  color: #333;
  border: 1px dashed #999;
  text-align: left;
}
.error-msg { color: red; font-weight: bold; }
ul { list-style-type: disc; padding-left: 20px; }
li { margin-bottom: 10px; display: flex; justify-content: space-between; align-items: center; }
.delete-btn, .edit-btn { margin-left: 5px; cursor: pointer; }
</style>