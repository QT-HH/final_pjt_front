<template>
  <div class="container">
    <h2>인기영화</h2>
    <carousel :loop="true"  :perPage="3"  :autoplayTimeout="3500" :paginationEnabled="false">
      <slide 
        v-for="(movie,idx) in movies"
        :movie="movie"
        :key="idx"
      >
        <div>
          <movie :movie="movie" @detailShow="detailShow"/>
        </div>
      </slide>
    </carousel>
    <v-row justify="center">
      <v-dialog
        v-model="dialog_detail"
        persistent
        max-width="600px"
      >
        <MovieDetail  :movie="selectedMovie" @close="close"/>
      </v-dialog>
    </v-row>
  </div>

</template>

<script>
import axios from 'axios'
import movie from './movie'
import MovieDetail from '../components/MovieDetail'
const SERVER_URL = process.env.VUE_APP_SERVER_URL
import {Carousel,Slide} from 'vue-carousel'

export default {
  name: 'movies',
  components: {
    movie,
    Carousel,
    Slide,
    MovieDetail,
  },
  data: function () {
    return {
      movies: [],
      detailSign:false,
      selectedMovie: {},
      dialog_detail: false,
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
    },
    detailShow: function (smovie) {
      this.selectedMovie = smovie
      this.dialog_detail = true
    },
    close: function () {
      this.dialog_detail = false
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