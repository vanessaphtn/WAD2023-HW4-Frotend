<template>
  <div class="form">
    <h3>Update Post</h3>
    <label for="body">Post Body</label>
    <textarea v-model="updatedBody" rows="4"></textarea>
    <div class="container">
      <button @click="updatePost" class="left">Update</button>
      <button @click="deletePost" class="right">Delete</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Post',
  data() {
    return {
      post: {},
      updatedBody: '',
    };
  },
  methods: {
    async fetchPost() {
      const postId = this.$route.params.postId;
      try {
        const response = await fetch(`http://localhost:3000/posts/${postId}`, {
          method: 'GET',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': 'Bearer ' + document.cookie.split('=')[1],
          },
          credentials: 'include',
        });

        if (response.ok) {
          const data = await response.json();
          this.post = data;
          this.updatedBody = data.body; // Set the initial value for the updatedBody
        } else {
          console.error('Failed to fetch post details');
        }
      } catch (error) {
        console.error(error);
      }
    },
    async updatePost() {
      const postId = this.$route.params.postId;
      try {
        const response = await fetch(`http://localhost:3000/posts/${postId}`, {
          method: 'PUT',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': 'Bearer ' + document.cookie.split('=')[1],
          },
          credentials: 'include',
          body: JSON.stringify({ body: this.updatedBody }),
        });

        if (response.ok) {
          console.log('Post updated successfully');
          // Refresh post details after update
          this.fetchPost();

          this.$router.push("/");
        } else {
          console.error('Failed to update post');
        }
      } catch (error) {
        console.error(error);
      }
    },
    async deletePost() {
      const postId = this.$route.params.postId;
      try {
        const response = await fetch(`http://localhost:3000/posts/${postId}`, {
          method: 'DELETE',
          headers: {
            'Authorization': 'Bearer ' + document.cookie.split('=')[1],
          },
          credentials: 'include',
        });

        if (response.ok) {
          console.log('Post deleted successfully');
          // Optionally, you can navigate back to the home page after deletion
          this.$router.push('/');
        } else {
          console.error('Failed to delete post');
        }
      } catch (error) {
        console.error(error);
      }
    },
    formatDate(dateString) {
      const date = new Date(dateString);
      return `${String(date.getDate()).padStart(2, '0')}-${String(date.getMonth() + 1).padStart(2, '0')}-${date.getFullYear()}`;
    },
  },
  mounted() {
    this.fetchPost();
  },
};
</script>


<style scoped>
.form {
  max-width: 420px;
  margin: 30px auto;
  background: rgb(216, 212, 212);
  text-align: left;
  padding: 40px;
  border-radius: 10px;
}
h3 {
  text-align: center;
  color: rgb(8, 110, 110);
}
label {
  color: rgb(8, 110, 110);
  display: inline-block;
  margin: 25px 0 15px;
  font-size: 0.8em;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}
textarea {
  display: block;
  padding: 10px 6px;
  width: 100%;
  box-sizing: border-box;
  border: none;
  border-bottom: 1px solid white;
}
button {
  background: rgb(8, 110, 110);
  border: 0;
  padding: 10px 20px;
  margin-top: 20px;
  color: white;
  border-radius: 20px;
  align-items: center;
  text-align: center;
}

.container{
  display: flex;
  justify-content: space-between;
}

.left{
  margin-right: auto;
}

.right{
  margin-left: auto;
}
</style>