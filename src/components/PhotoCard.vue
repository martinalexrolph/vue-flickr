<template>
  <div class="photo-card">
    <div class="photo-card__photo-container">
      <img class="photo-card__photo" v-bind:src="photo.media.m">
    </div>
    <div class="photo-card__text-container">
      <h3 class="photo-card__header"><a v-bind:href="photo.link" class="photo-card__title">{{ getTitle() }}</a> by <a v-bind:href="getAuthorLink()">{{ getAuthor() }}</a></h3>
      <p><b>Description:</b> {{ getDescription() }}</p>
      <p v-if="getTags()"><b>Tags:</b> {{ getTags() }}</p>
    </div>
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
      return span.textContent || span.innerText
    },
    getTags: function () {
      const tagsArray = this.photo.tags.split(' ')
      return tagsArray.join(', ')
    },
    getAuthorLink: function () {
      // there's no author link included in the object so get it from the photo link
      return this.photo.link.split('/').slice(0, -2).join('/')
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
  margin: 10px;
  overflow: scroll;
  box-shadow: 1px 1px 5px #bbb;
}

.photo-card__photo-container {
  width: 100%;
  height: 180px;
  display: flex;
  //background: #444;
  align-items: center;
  justify-content: center;
}

.photo-card__text-container {
  padding: 10px;
  text-align: left;
  font-size: 13px;
  height: 100px;
  overflow: scroll;
}

.photo-card__photo {
  max-width: 90%;
  max-height: 150px;
}

.photo-card__header {
  font-size: 13px;
  margin-top: 0;
}
</style>
