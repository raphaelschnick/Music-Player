<template>
  <div id="app">
    <player></player>
  </div>
</template>

<script>
import Player from './components/Player.vue'
export default {
  name: 'App',
  components: {
    Player
  }
}
</script>

<style>
html {
  scroll-behavior: smooth;
}
</style>
