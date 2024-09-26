<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <div class="app-container">
    <div class="app-side">
      <header>
        <div class="wrapper">
          <HelloWorld msg="Online Photo Filters" />
          <nav>
            <RouterLink to="/">Home</RouterLink>
            <RouterLink to="/about">About</RouterLink>
          </nav>
        </div>
      </header>
      <RouterView />
    </div>
    <div class="ad-side">Ads here</div>
  </div>
</template>

<style>
body {
  background: #111;
  color: #777;
}
header {
  text-align: center;
}
a {
  text-decoration: none;
  margin: 1em;
  color: #777;
}

.app-container {
  height: 100vh;
  max-width: 100vw;
  display: grid;
  grid-template-areas: 'app ads';
  grid-template-columns: calc(100% - 200px) 200px;
  grid-template-rows: 1fr;
}
.app-side {
  grid-area: app;
  position: relative;
}
.ad-side {
  grid-area: ads;
}
</style>
