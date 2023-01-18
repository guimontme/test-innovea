<script setup lang="ts">
import { ref } from 'vue';
import AppHeader from './components/AppHeader.vue'

const baseApi = 'https://newsapi.org/v2/';
const apiKey = '1c90209a6d4d481c979844d2161861aa';
const queue = 'javascript';
const language = 'pt'

interface Article {
  title: string
  author: string
  description: string
  url: string
}

const dataNews = ref<Array<Article>>([])

const loadDataNews = async () => {
  dataNews.value = await fetch(`${baseApi}everything?q=${queue}&apiKey=${apiKey}&language=${language}`)
  .then((res) => res.json())
  .then((data) => {
    console.log(data.articles)
   return data.articles
  }
  )
};

loadDataNews();
</script>

<template>
  <AppHeader />
  <main>
    <article class="article" v-for="dataNew in dataNews" :key="dataNew.url">
      <h2>{{ dataNew.title }}</h2>
      <small><strong>By: </strong>{{ dataNew.author ?? 'Unknown' }}</small>
      <p >{{ dataNew.description }}</p>
      <footer class="footer__article">
        <a :href="dataNew.url" target="_blank" class="read_more">read +</a>
      </footer>
    </article>
  </main>
</template>

<style scoped>
.article {
  margin-bottom: 2rem;
  text-align: left;
  padding-bottom: 1.5rem;
  border-bottom: 1px solid #bbb;
}
.article h2 {
  margin-bottom: 5px;
}
.article small {
  color: #442;
}
.article .footer__article {
  text-align: right;
}
.article .footer__article .read_more{
  color: #059;
}
</style>
