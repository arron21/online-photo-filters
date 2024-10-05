<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'

const date = new Date().getFullYear();
</script>

<template>
  <div class="app-container">
    <div class="app-side">
      <header>
        <div class="wrapper">
          <HelloWorld msg="Online Photo Filters" />
          <nav>
            <ul>
              <li>
                <RouterLink to="/">Home</RouterLink>
              </li>
              <li>
                <RouterLink to="/about">About</RouterLink>
              </li>
            </ul>
          </nav>
        </div>
      </header>
      <main>
        <RouterView />
      </main>
    </div>
    <div class="ad-side"></div>
    <footer>
      <p>Made with ❤️ and ☕</p><p>Arron McCrory</p>
    </footer>
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
nav ul {
  all: unset;
  list-style-type: none;
  display: flex;
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
  grid-template-areas: 
    'app ads'
    'footer footer';
  grid-template-columns: calc(100% - 200px) 200px;
  grid-template-rows: 1fr 150px;
  overflow-y: auto;
}
@media screen and (max-width: 1024px) {
  .app-container {
    grid-template-areas: 
    'ads' 
    'app'
    'footer';
    grid-template-columns: 1fr;
    grid-template-rows: 150px 1fr;
  }
}
.app-side {
  grid-area: app;
  position: relative;
}
.ad-side {
  grid-area: ads;
}
footer {
  align-content: center;
  text-align: center;
  height: 150px;
  display: block;
}
</style>
