<template>
  <div class="text-center">
    <img :src=imgSrc alt="sadf" style="width:200px; height:300px;">
    <br>
    {{movie.title}}
  </div>
</template>

<script>

export default {
  name:'movie',
  components: {
  },
  props: {
    movie: [Object],
  },
  computed: {
    imgSrc: function () {
      return `https://image.tmdb.org/t/p/w500${this.movie.poster_path}`
    }
  }
}
</script>

<style>

</style>