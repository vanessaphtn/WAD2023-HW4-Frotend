
<template>
  <div class="form">
    <h3>A Post</h3>
      <label for="body">Post Body</label>
      <textarea name="body" required v-model='body'></textarea>
    <div class="container">
      <button @click="updatePost()"  class="center">Update</button>
      <button @click="deletePost()" class="center">Delete</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Post',
  data: function (){
    return {
      body: '',
    }
  },
  methods:{
    deletePost(){
      fetch('http://your-api-url/posts/${postId}',{
        method: 'DELETE',
        credentials: 'include',
      })
    },
    updatePost(){
      var data = {
        body: this.body,
        date: new Date().toISOString()
      };
      fetch("http://localhost:3000/posts",{
        method: 'PUT',
        credentials: 'include',
      })
    }
  }
}
</script>

<style scoped>
h3 {
  text-align: center;
  color: rgb(8, 110, 110);
}
.form {
  max-width: 420px;
  margin: 30px auto;
  background: rgb(216, 212, 212);
  text-align: left;
  padding: 40px;
  border-radius: 10px;
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
.center {
  margin: auto;
  border: 0;
  padding: 10px 20px;
  margin-top: 20px;
  width: 30%;
}
.container {
  display: flex;
  justify-content: center;
}
</style>