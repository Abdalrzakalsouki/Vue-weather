<script setup>
import { RouterLink, RouterView } from "vue-router";
</script>

<template>
  <nav class="navbar navbar-dark bg-dark">
    <a class="navbar-brand" href="#"
      ><spane class="title-word">Vue.js</spane> Weather Application</a
    >
    <RouterLink to="/" class="nav-item nav-link links">Home</RouterLink>
    <RouterLink to="/about" class="nav-item nav-link links">About</RouterLink>
  </nav>
  <RouterView />
  <main>
    <div class="container-fluid container">
      <h1 class="text-center">
        Welcome to <span class="title-word">Vue.js</span> Weather Application
      </h1>
    </div>
  </main>
</template>

<style>
.container {
  padding-top: 80px;
  padding-bottom: 50px;
}
.links {
  color: #fff;
  font-size: 20px;
}

.links:hover {
  background-color: #fefefe;
  color: black;
  transition: 0.3s;
  border-radius: 5%;
}

.title-word {
  color: #4caf50;
}
</style>
