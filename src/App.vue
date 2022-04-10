<script setup>
import { provide, onMounted } from "vue";
import useWorldStore  from "./store/world";
import HelloWorld from './components/HelloWorld.vue'

const { clue1, fetchTodos, selectedClue, loading } = useWorldStore();

onMounted(() => {
  fetchTodos();
});

</script>

<template>
  <HelloWorld msg="Hello Vue 3 + Vite" />
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
