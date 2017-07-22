<template>
  <div class="photo-feed">
    <photo-card
      v-for="(photo, index) in photos"
      v-bind:photo="photo"
      v-bind:key="index">
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
  data: () => ({
    photos: []
  }),
  created () {
    $.ajax({
      url: 'https://api.flickr.com/services/feeds/photos_public.gne?format=json&jsoncallback=?',
      dataType: 'json'
    }).then((data) => {
      this.photos = data.items
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.photo-feed {
  display: flex;
  flex-wrap: wrap;
}

</style>
