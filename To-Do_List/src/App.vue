<template>
  <div id="app">
    <ToDoList />
  </div>
</template>

<script setup>
import ToDoList from './components/ToDoList.vue';
</script>

<style>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  background-color: #f9f9f9;
}

h1 {
  text-align: center;
  color: #333;
  margin: 1rem 0;
}
</style>
