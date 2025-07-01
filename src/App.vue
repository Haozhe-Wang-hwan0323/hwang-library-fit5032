<script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
import JSON from './components/JSON.vue';
</script>

<template>
  <div class="container mx-auto p-4">
    <JSON />
  </div>
</template>


<style>
body {
  font-family: Arial, sans-serif;
  background-color: #f8fafc;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
}
</style>