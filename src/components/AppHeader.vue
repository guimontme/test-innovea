<script setup lang="ts">
const title = import.meta.env.VITE_APP_TITLE;
</script>

<template>
  <header class="app_header">
    <h1 class="app_header__title">{{ title }}</h1>
    <p class="app_header__subtitle">Everything about Technology</p>
  </header>
</template>

<style>
.app_header {
  border-bottom: 1px solid #000;
  padding: 20px;
  text-align: center;
}
.app_header__title {
  margin-bottom: 10px;
  border-bottom: 1px solid #000;
  display: inline-block;
  margin: auto;
}
.app_header__subtitle {
  text-transform: uppercase;
  color: #999;
  font-size: .9rem;
}
</style>