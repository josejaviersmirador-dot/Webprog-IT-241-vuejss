<script>
import axios from 'axios';

const apiClient = axios.create({
  baseURL: import.meta.env.VITE_API_URL || 'https://your-vercel-project.vercel.app/api',
  headers: {
    'Content-Type': 'application/json'
  }
});

export default {
  data() {
    return {
      posts: [],
      loading: true,
      error: null,
      newPost: {
        title: '',
        body: ''
      }
    };
  },
  methods: {
    async fetchPosts() {
      try {
        const response = await apiClient.get('/posts');
        this.posts = response.data;
      } catch (err) {
        this.error = "Connection to Vercel failed: " + err.message;
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
        this.error = "Update failed: " + err.message;
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