<script setup>
import Scoreboard from './components/Scoreboard.vue'
</script>

<template>
  <div class="container">
    <h1>Scoreboard</h1>
    <Scoreboard />
  </div>
</template>

<style>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
}

h1 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 2rem;
}
</style>