<template>
  <div class="card">
    <div class="photo-container" v-on:click="modalOpen = true">
      <img class="photo" v-bind:src="photo.media.m">
    </div>
    <div class="text-container">
      <a class="title" v-bind:href="photo.link">
        {{ getTitle() }}
      </a>
      <p class="author">
        by <a class="author-link" v-bind:href="getAuthorLink()">{{ getAuthor() }}</a>
      </p>
      <p v-if="getDescription()"><b>Description:</b> {{ getDescription() }}</p>
      <p v-if="getTags()"><b>Tags:</b> {{ getTags() }}</p>
    </div>
    <modal v-if="modalOpen" v-on:close="modalOpen = false" v-bind:photo="photo"></modal>
  </div>
</template>

<script>
import $ from 'jquery'
import Modal from './Modal'

export default {
  name: 'card',
  data () {
    return {
      modalOpen: false
    }
  },
  components: {
    Modal
  },
  props: [
    'photo'
  ],
  methods: {
    getTitle: function () {
      // some photos have very long titles and some don't have any so trim and give a fallback
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
      // the description field is HTML, and includes an <img> tag of the photo itself, so extract the actual text using a virtual DOM element
      var span = document.createElement('span')
      span.innerHTML = this.photo.description
      const contents = span.textContent || span.innerText
      // ignore the 'username posted a photo:' section
      return $.trim(contents.split('posted a photo:')[1])
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
.card {
  width: 300px;
  height: 380px;
  max-width: 100%;
  flex-grow: 1;
  margin: 10px;
  overflow: scroll;
  box-shadow: 1px 1px 6px #bbb;
  transition: box-shadow 0.2s ease-in-out;
  border-radius: 3px;
  background: #f1f1f1;
}

.photo-container {
  width: 100%;
  height: 260px;
  display: flex;  align-items: center;
  justify-content: center;
  border-bottom: 1px solid #ddd;
  background: white;
}

.text-container {
  padding: 10px 10px 0 10px;
  text-align: left;
  font-size: 13px;
  height: 109px;
  overflow: scroll;
}

.photo {
  max-width: 90%;
  max-height: 200px;
  transition: transform 0.2s ease-in-out;
}

.photo-container:hover .photo {
  transform: scale(1.07);
}

.title {
  font-weight: 900;
  font-size: 20px;
  color: #333;
  text-decoration: none;
  margin-bottom: 0;
  margin-top: 0;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  display: block;
}

.title:hover {
  text-decoration: underline;
}

.author {
  margin-top: 5px;
  font-size: 15px;
}

.author-link {
  font-weight: 900;
  color: #333;
  text-decoration: none;
}

.author-link:hover {
  text-decoration: underline;
}
</style>
