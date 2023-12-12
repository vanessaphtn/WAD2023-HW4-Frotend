<template>
    <div class="form">
      <h3>Add Post</h3>
      <label for="body">Post Body</label>
      <textarea name="body" required v-model="body"></textarea>
      <button @click="addPost" class="AddPost">Add Post</button>
    </div>
  </template>
  
  <script>
  export default {
    name: "AddPost", 
  
    data: function() {
      return {
        body: '',
      }
    },
  
    methods: {

      addPost() {
        var data = {
          body: this.body,
          date: new Date().toISOString()
        };
  
        fetch("http://localhost:3000/posts", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            'Authorization': 'Bearer ' + document.cookie.split('=')[1]  // Add this line
          },
          credentials: 'include',
          body: JSON.stringify(data),
        })
        .then((response) => response.json())
        .then((data) => {
          console.log(data);
          this.$router.push("/");
        })
        .catch((e) => {
          console.log(e);
          console.log("error");
        });
      },
    }, 
  }
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
  </style>
  