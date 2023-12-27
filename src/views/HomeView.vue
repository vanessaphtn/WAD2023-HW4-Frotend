<template>
  <div class="header">
    <div class="container">
      <button v-if="authResult" @click="Logout" class="center">Logout</button>
    </div>
    <router-link v-for="post in posts" :key="post.id" :to="{ name: 'Post', params: { postId: post.id } }">
      <div class="post">
        <p class="date">{{ formatDate(post.date) }}</p>
        <p><b>{{ post.body }}</b></p>
      </div>
    </router-link>
    <div class="container">
      <button v-if="authResult" @click="AddPost" class="center">Add Post</button>
      <button v-if="authResult" @click="DeleteAll" class="center">Delete All</button>
    </div>
  </div>
</template>


<script>
// @ is an alias to /src
import auth from "../auth";

export default {
  name: "HomeView", 
  components: {
  },
   data: function() {
    return {
    posts:[ ],
    authResult: auth.authenticated(),
    }
  },
  methods: {
    Logout() {
      fetch("http://localhost:3000/auth/logout", {
          credentials: 'include', //  Don't forget to specify this if you need cookies
      })
      .then((response) => response.json())
      .then((data) => {
        console.log(data);
        console.log('jwt removed');
        //console.log('jwt removed:' + auth.authenticated());
        this.$router.push("/login");
        //location.assign("/");
      })
      .catch((e) => {
        console.log(e);
        console.log("error logout");
      });
    },
    AddPost() {
      this.$router.push("/addpost");
    },
    DeleteAll() {
      fetch("http://localhost:3000/posts", {
          method: 'DELETE',
          credentials: 'include', //  Don't forget to specify this if you need cookies
      })
      .then((response) => {
        response.json()
        console.log(response)
      })
      .then((data) => {
        console.log(data);
        console.log('All posts deleted');
        this.posts = [];
      })
      .catch((e) => {
        console.log(e);
        console.log("error deleting all posts");
      });
    },
    
  formatDate(dateString) {
  const date = new Date(dateString);
  return `${String(date.getDate()).padStart(2, '0')}-${String(date.getMonth() + 1).padStart(2, '0')}-${date.getFullYear()}`;
},
  }, 
  mounted() {
    fetch('http://localhost:3000/posts', {
    method: "GET",
    headers: {
      "Content-Type": "application/json",
      'Authorization': 'Bearer ' + document.cookie.split('=')[1]  // Add this line
    },
    credentials: 'include',
  })
        .then((response) => response.json())
        .then(data => this.posts = data)
        .catch(err => console.log(err.message))
    }
};
</script>

<style scoped>
body{
  margin: 20px 40px;
  font-size: 1.2rem;
  letter-spacing: 1px;
  background: #fafafa;
  position: relative;
}
.post-list{
  margin-bottom: 5px;
  padding: 3px 5px;
}
.date{
    margin: 0;
  padding: 0;
  font-family: 'Quicksand', sans-serif;
  color: #444;
  text-align: right;
}

h1, h2, h3, h4, ul, li, a, input, label, button, div, footer{
  margin: 0;
  padding: 0;
  font-family: 'Quicksand', sans-serif;
  color: #444;
}
nav{
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  margin-bottom: 80px;
}
input{
  padding: 10px 12px;
  border-radius: 4px;
  border: 1px solid #ddd;
  font-size: 1em;
  width: 100%;
}
label{
  display: block;
  margin: 20px 0 10px;
}
button{
  margin-top: 30px;
  border-radius: 36px;
  background: #FEE996;
  border:0;
  font-weight: 700;
  font-size: 0.8em;
  display: block;
  padding: 10px 16px;
  letter-spacing: 2px;
}
nav{
  display: flex;
  align-items: center;
}
.post {
  background: rgb(189, 212, 199);
  border-radius: 10px;
    width: 80%;
    position: relative;
    padding: 20px;
    margin: 5px auto;
    text-align: left;
}
.center {
  margin: auto;
  border: 0;
  padding: 10px 20px;
  margin-top: 20px;
  margin: 10px auto;
  width: 30%; 
}
.container {
  display: flex;
  justify-content: center;
}
</style>