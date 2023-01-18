<script setup lang="ts">
import { ref, onMounted, nextTick } from 'vue';
import AppHeader from './components/AppHeader.vue'

const baseApi = 'https://newsapi.org/v2/';
const apiKey = import.meta.env.VITE_APP_API_KEY;
const queue = 'tecnologia';
const language = 'pt'

interface Article {
  title: string
  author: string
  description: string
  url: string
}

const dataNews = ref<Array<Article>>([])
const page = ref<number>(1)
const showLoadMore = ref<boolean>(true);

const loadDataNews = async () => {
  await fetch(`${baseApi}everything?q=${queue}&apiKey=${apiKey}&language=${language}&pageSize=20&page=${page.value}`)
  .then((res) => res.json())
  .then((data) => {
    if(data.status == 'ok') {
      dataNews.value.push(...data.articles)
      if(dataNews.value.length >= 100) showLoadMore.value = false;
    } 
  })
  .catch(error => {
    showLoadMore.value = false;
  })
};

function loadMore() {
  page.value++;
  loadDataNews(); 
}

onMounted( loadDataNews );

</script>

<template>
  <AppHeader />
  <main>
    <TransitionGroup name="articles">
      <article class="article" v-for="dataNew in dataNews" :key="dataNew.url">
        <h2>{{ dataNew.title }}</h2>
        <small><strong>By: </strong>{{ dataNew.author ?? 'Unknown' }}</small>
        <p >{{ dataNew.description }}</p>
        <footer class="footer__article">
          <a :href="dataNew.url" target="_blank" class="read_more">read +</a>
        </footer>
      </article>
    </TransitionGroup>
  </main>
  <footer class="app_footer">
    <div v-if="showLoadMore">
      <button @click="loadMore" class="">LOAD MORE</button>
    </div>
  </footer>
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
.articles-enter-active,
.articles-leave-active {
  transition: all 0.5s ease;
}
.articles-enter-from,
.articles-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
.app_footer {
  padding-top: 20px;
}
</style>
