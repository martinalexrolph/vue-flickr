<template>
  <div id="app">
    <photo-feed></photo-feed>
  </div>
</template>

<script>
import PhotoFeed from './components/PhotoFeed'

export default {
  name: 'app',
  components: {
    PhotoFeed
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
