<template>
  <div class="photo-feed">
    <h1>{{ msg }}</h1>

    <photo-card
      v-for="photo in photos"
      v-bind:photo="photo">
    </photo-card>
  </div>
</template>

<script>
import $ from 'jquery'

import PhotoCard from './PhotoCard'

export default {
  name: 'photo-feed',
  components: {
    PhotoCard
  },
  data () {
    return {
      msg: 'Flickr Photo Feed',
      photos: []
    }
  },
  created () {
    $.ajax({
      url: 'https://api.flickr.com/services/feeds/photos_public.gne?format=json&jsoncallback=?',
      dataType: 'json'
    }).then((data) => {
      console.log(data)
      this.photos = data.items
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
