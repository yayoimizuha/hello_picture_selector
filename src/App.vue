<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/>
  <csv_opener asset="../assets/embeddings.csv"/>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import csv_opener from "@/components/csv_opener";

export default {
  name: 'App',
  components: {
    csv_opener,
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2873c1;
  margin-top: 60px;
}
</style>