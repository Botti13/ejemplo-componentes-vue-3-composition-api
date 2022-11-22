<script setup>
import { ref, onMounted } from 'vue';

import BlogPost from './components/BlogPost.vue'
import PaginatePost from './components/PaginatePost.vue'
import LoadingSpinner from './components/LoadingSpinner.vue'

const posts = ref([])
const postxPage = 10
const inicio = ref(0)
const fin = ref(postxPage)
const loading = ref(true)

const favorito = ref('')

const cambiarFavorito = (title) => {
  favorito.value = title
}

const next = () => {
  inicio.value = inicio.value + postxPage
  fin.value = fin.value + postxPage
}

const prev = () => {
  inicio.value += - postxPage
  fin.value += - postxPage
}

// onMounted(async() => {
// // loading.value = true

// })

// fetch('https://jsonplaceholder.typicode.com/posts')
//   .then(res => res.json())
//   .then((data) => { posts.value = data })
//   .catch(e => console.log(e))
//   .finally(() => loading.value = false)

const fetchData = async () => {
  try {
    const res = await fetch('https://jsonplaceholder.typicode.com/posts')
    posts.value = await res.json()
  } catch (error) {
    console.log(error)
  } finally {
    setTimeout(() => {
    loading.value = false
    }, 2000)
  }

}
fetchData()
</script>

<template>
  <LoadingSpinner v-if="loading" />
  <div class="container" v-else>
    <h1>APP</h1>
    <h2>Mi Post Favorito: {{ favorito }}</h2>


    <PaginatePost @next='next' @prev='prev' :inicio="inicio" :fin="fin" :maxLength="posts.length" class="mb-2">
    </PaginatePost>

    <BlogPost v-for="post in posts.slice(inicio, fin)" :key="post.id" :title="post.title" :id="post.id"
      :body="post.body" @cambiarFavoritoNombre="cambiarFavorito" class="mb-2"></BlogPost>




  </div>

</template>

