<script setup lang="ts">
defineProps<{
  msg: string
}>()
</script>

<template>
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
  text-align: center;

  margin: 1rem;
  background: linear-gradient(90deg, rgba(150,48,30,1) 0%,rgba(199,201,88,1) 10%,rgba(28,147,46,1) 20%,rgba(74,165,168,1) 30%,rgba(89,90,165,1) 40%,rgba(84,16,67,1) 50%,rgba(28,58,63,1) 60%,rgba(82,175,183,1) 70%,rgba(78,170,76,1) 80%,rgba(150,48,30,1) 90%,rgba(199,201,88,1) 100%);
  background-size: 1000% 100%;
  
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  animation: background 20s linear infinite;

}
@keyframes background {
  0%{background-position:0% 50%}
  100%{background-position:100% 50%}
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}
</style>
