<script setup>
import { onMounted, ref } from "vue";

import BlogPost from "./components/BlogPost.vue";
import PaginatePost from "./components/PaginatePost.vue";
import LoadingSpinner from "./components/LoadingSpinner.vue";


const posts = ref([]);
const postXpage = 10;
const inicio = ref(0);
const fin = ref(postXpage);
const loading = ref(true);

const favorito = ref("")

const cambiarFavorito = (title) => {
  favorito.value = title;
};

const next = () => {
  inicio.value += + postXpage;
  fin.value += + postXpage;
}
const previo = () => {
  inicio.value += - postXpage;
  fin.value += - postXpage;
}

onMounted(async()=>{
  loading.value=true;
  try{
    const res = await  fetch('https://jsonplaceholder.typicode.com/posts');
    posts.value = await res.json();
  } catch (error){
    console.log(error);
  }
  finally {
    setTimeout(()=> {
      loading.value=false;
    },2000);
  }
});

/* fetch('https://jsonplaceholder.typicode.com/posts')
  .then(res => res.json() )
  .then(data=> {
    posts.value = data})
  .finally(()=>{
    setTimeout(()=>{
      loading.value=false
    },2000)
    }) */

</script>

<template>
  <LoadingSpinner v-if="loading"/>
  <div class="container" v-else="">
    <h1>APP</h1>
    <h2>Mi post favorito: {{ favorito }}</h2>


      
    <PaginatePost
      class="mb-2"
      @next="next"
      @previo="previo"
      :inicio="inicio"
      :fin="fin"
      :maxLength="posts.length"
    />
    
    <BlogPost
    v-for="post in posts.slice(inicio, fin)"
    :key="post.id"
    :title="post.title"
    :id="post.id"
    :body="post.body"
    @cambiarFavoritoNombre="cambiarFavorito"
    class="mb-2"
    ></BlogPost>  
   
  </div>
 
</template>

<style>

</style>