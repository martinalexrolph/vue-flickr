<template>
  <div id="app">
    <h1>Flickr's latest photos</h1>
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
@import url('https://fonts.googleapis.com/css?family=Playfair+Display:400,900');

#app {
  font-family: 'Playfair Display', serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #333;
}
</style>
