<script>
import axios from 'axios';

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
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=5');
        this.posts = response.data;
      } catch (err) {
        this.error = "Error fetching posts: " + err.message;
      } finally {
        this.loading = false;
      }
    },

    async addPost() {
      try {
        const response = await axios.post('https://jsonplaceholder.typicode.com/posts', this.newPost);
        this.posts.unshift(response.data);
        this.newPost = { title: '', body: '' };
      } catch (err) {
        this.error = "Error adding post: " + err.message;
      }
    },

    async deletePost(id) {
      try {
        await axios.delete(`https://jsonplaceholder.typicode.com/posts/${id}`);
        this.posts = this.posts.filter(post => post.id !== id);
      } catch (err) {
        this.error = "Error deleting post: " + err.message;
      }
    },

    editPost(post) {
      this.newPost = { ...post };
    }
  },
  mounted() {
    this.fetchPosts();
  }
};
</script>
