<script setup>
import { RouterLink, RouterView } from "vue-router";
import { onMounted } from "vue";
import NavBar from "./components/NavBar.vue";
// import CountriesList from "./components/CountriesList.vue";
</script>

<template>
  <div class="app">
    <NavBar />
    <RouterView />
  </div>
  <!-- <CountriesList :countries="countries" /> -->
</template>

<style scoped></style>
