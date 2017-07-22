<template>
  <div class="photo-card">
    <div class="photo-card__photo-container">
      <img class="photo-card__photo" v-bind:src="photo.media.m">
    </div>
    <h3 class="photo-card__header"><a v-bind:href="photo.link" class="photo-card__title">{{ getTitle() }}</a> by {{ getAuthor() }}</h3>
    <div>{{ getDescription() }}</div>
  </div>
</template>

<script>
import $ from 'jquery'

export default {
  name: 'photo-feed',
  data () {
    return {
      msg: "It's a photo!"
    }
  },
  props: [
    'photo'
  ],
  methods: {
    getTitle: function () {
      const trimmed = $.trim(this.photo.title)
      if (!trimmed) {
        return 'Untitled'
      } else if (trimmed.length > trimmed.substr(0, 50).length) {
        return trimmed.substr(0, 50) + '...'
      } else {
        return trimmed
      }
    },
    getAuthor: function () {
      if (!this.photo.author) {
        return 'Anon'
      }
      // the author field typically looks like 'nobody@flickr.com ("matmpimentel")' with the same email address each time
      const authorArray = this.photo.author.split(/\("|"\)/)
      if (authorArray.length === 3) {
        try {
          // unescape() is deprecated and so may not work, so keep a fallback.
          return unescape(authorArray[1])
        } catch (e) {
          return authorArray[1]
        }
      } else {
        return this.photo.author
      }
    },
    getDescription: function () {
      var span = document.createElement('span')
      span.innerHTML = this.photo.description
      console.log(this.photo.description)
      return span.textContent || span.innerText
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.photo-card {
  width: 250px;
  height: 300px;
  max-width: 100%;
  flex-grow: 1;
  margin: 5px;
  padding: 10px;
  border: 1px solid #ccc;
  overflow: scroll;
}

.photo-container {
  width: 100%;
  height: 150px;
  display: flex;
  background: grey;
}

.photo-card__photo {
  max-width: 100%;
  max-height: 150px;
}

.photo-card__header {
  font-size: 13px;
}
</style>
