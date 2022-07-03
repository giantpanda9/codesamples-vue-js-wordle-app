<template>
    <WordleApp />
</template>

<script>
import WordleApp from './components/Wordle.vue'

export default {
  name: 'App',
  components: {
    WordleApp
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
