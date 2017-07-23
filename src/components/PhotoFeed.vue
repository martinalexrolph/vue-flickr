<template>
  <div class="photo-feed">
    <div class="header">
      <h1 class="title">Flickr's latest photos</h1>
      <div class="search-box">
        <input
          class="search-input"
          type="text"
          placeholder="Search tags"
          v-model="tags"
          v-on:keyup.enter="searchTags">
        <button
          class="search-button"
          v-on:click="searchTags">Search</button>
      </div>
    </div>
    <div class="grid"> 
      <photo-card
        v-for="(photo, index) in photos"
        v-bind:photo="photo"
        v-bind:key="index">
      </photo-card>
    </div>
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
    photos: [],
    tags: ''
  }),
  created () {
    $.ajax({
      url: 'https://api.flickr.com/services/feeds/photos_public.gne?format=json&jsoncallback=?',
      dataType: 'json'
    }).then((data) => {
      this.photos = data.items
    })
  },
  methods: {
    searchTags: function () {
      const tags = this.tags.split(/ |,/).map($.trim).filter((el) => !!el.length).join(',')
      $.ajax({
        url: `https://api.flickr.com/services/feeds/photos_public.gne?tags=${tags}&format=json&jsoncallback=?`,
        dataType: 'json'
      }).then((data) => {
        this.photos = data.items
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.photo-feed {
  text-align: left;
}

.header {
  padding: 10px;
  display: flex;
  justify-content: space-between;
}



.title {
  margin: 0;
  flex-grow: 1;
}

.search-box {
  display: flex;
  min-height: 43px;
  padding: 3px 0;
  flex-grow: 1;
}

.search-input {
  font-family: 'Playfair Display', serif;
  box-sizing: border-box;
  font-size: 16px;
  display: block;
  padding: 3px 8px;
  margin-right: 10px;
  border-radius: 3px;
  border: 1px solid #ccc;
  flex-grow: 1;
}

.search-button {
  font-family: 'Playfair Display', serif;
  font-weight: 900;
  font-size: 16px;
  display: block;
  padding: 3px 8px;
  border-radius: 3px;
  border: 1px solid #888;
  background-color: #f1f1f1;
  color: #333;
}

.grid {
  display: flex;
  flex-wrap: wrap;
}

@media (max-width: 700px) {
  .header {
    flex-direction: column;
  }

  .title {
    margin-bottom: 10px;
  }
}

</style>
