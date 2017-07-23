<template>
  <div id="app">
    <feed></feed>
  </div>
</template>

<script>
import Feed from './components/Feed'

export default {
  name: 'app',
  components: {
    Feed
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Playfair+Display:400,900');

#app {
  font-family: 'Playfair Display', serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #333;
}
</style>
