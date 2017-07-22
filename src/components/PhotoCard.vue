<template>
  <div class="photo-card">
    <h1>{{ photo.title }}</h1>
  </div>
</template>

<script>
export default {
  name: 'photo-feed',
  data () {
    return {
      msg: "It's a photo!"
    }
  },
  props: [
    'photo'
  ]
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
