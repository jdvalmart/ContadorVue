<script setup>
import BlogPost from "./components/BlogPost.vue"
import { ref } from "vue";

const posts = ref([

]);
const favorito = ref("")

const cambiarFavorito = (title) => {
  favorito.value = title;
};

fetch('https://jsonplaceholder.typicode.com/posts')
  .then(res => res.json() )
  .then(data=> {
    posts.value = data})

</script>

<template>
  <div class="container">
    <h1>APP</h1>
    <h2>Mi post favorito: {{ favorito }}</h2>

    <BlogPost
    v-for="post in posts"
    :key="post.id"
    :title="post.title"
    :id="post.id"
    :body="post.body"
    @cambiarFavoritoNombre="cambiarFavorito"
    ></BlogPost>  
   
  </div>
 
</template>

<style>

</style>