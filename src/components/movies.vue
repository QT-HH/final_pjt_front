<template>
  <div class="container">
    <h2>인기영화</h2>
    <carousel :loop="true"  :perPage="3" :autoplay="true" :autoplayTimeout="3500" :paginationEnabled="false">
      <slide 
        v-for="(movie,idx) in movies"
        :movie="movie"
        :key="idx"
      >
        <div>
          <movie :movie="movie" />
        </div>
      </slide>
    </carousel>
    
  </div>

</template>

<script>
import axios from 'axios'
import movie from './movie'
const SERVER_URL = process.env.VUE_APP_SERVER_URL
import {Carousel,Slide} from 'vue-carousel'

export default {
  name: 'movies',
  components: {
    movie,
    Carousel,
    Slide,
  },
  data: function () {
    return {
      movies: [],
    }
  },
  created: function () {
    axios.get(`${SERVER_URL}/movies/`)
      .then((res) => {
        console.log(res)
        this.movies = res.data
      })
      .catch((err) => {
        console.error(err)
      })
  },
  methods: {
    moviePoster: function (movie) {
      return `https://image.tmdb.org/t/p/w500${movie.poster_path}`
    }
  }
}
</script>

<style>
  .example-slide {
    align-items: center;
    background-color: #666;
    color: #999;
    display: flex;
    font-size: 1.5rem;
    justify-content: center;
    min-height: 10rem;
  }
</style>