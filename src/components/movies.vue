<template>
  <div>
    <v-card
      elevation="24"
      shaped
      class="p-4"
    >
      <h2>Top 100</h2>
      <carousel :autoplay="true" :loop="true"  :perPage="7"  :autoplayTimeout="3500" :paginationEnabled="false" :perPageCustom="[[480, 2], [768, 3],[960,4],[1264,5],[1904,7]]">
        <slide 
          v-for="(movie,idx) in movies"
          :movie="movie"
          :key="idx"
        >
          <div class="px-2">
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
    </v-card>
    <br><br>

    <v-card
      elevation="24"
      shaped
      class="p-4"
    >
      <h2>New 100</h2>
      <carousel :autoplay="true" :loop="true"  :perPage="7"  :autoplayTimeout="3500" :paginationEnabled="false" :perPageCustom="[[480, 2], [768, 3],[960,4],[1264,5],[1904,7]]">
        <slide 
          v-for="(movie,idx) in movies2"
          :movie="movie"
          :key="idx"
        >
          <div class="px-2">
            <movie_recent :movie="movie" @detailShow2="detailShow2"/>
          </div>
        </slide>
      </carousel>
      <v-row justify="center">
        <v-dialog
          v-model="dialog_detail2"
          persistent
          max-width="600px"
        >
          <MovieDetail  :movie="selectedMovie2" @close="close2"/>
        </v-dialog>
      </v-row>
    </v-card>
    <br><br>
  </div>

</template>

<script>
import axios from 'axios'
import movie from './movie'
import movie_recent from './movie_recent'
import MovieDetail from '../components/MovieDetail'
const SERVER_URL = process.env.VUE_APP_SERVER_URL
import {Carousel,Slide} from 'vue-carousel'

export default {
  name: 'movies',
  components: {
    movie,
    movie_recent,
    Carousel,
    Slide,
    MovieDetail,
  },
  data: function () {
    return {
      movies: [],
      movies2: [],
      
      detailSign:false,
      detailSign2:false,
      selectedMovie: {},
      selectedMovie2: {},
      dialog_detail: false,
      dialog_detail2: false,

      login:false,
    }
  },
  created: function () {
    axios.get(`${SERVER_URL}/movies/`)
      .then((res) => {
        this.movies = res.data
      })
      .catch((err) => {
        console.error(err)
      })

    axios.get(`${SERVER_URL}/movies/recent/`)
      .then((res) => {
        this.movies2 = res.data
      })
      .catch((err) => {
        console.error(err)
      })

    const token = localStorage.getItem('jwt')

    if (token) {
      this.login = true
    }
  },
  methods: {
    detailShow: function (smovie) {
      this.selectedMovie = smovie
      this.dialog_detail = true
    },
    close: function () {
      this.dialog_detail = false
    },
    detailShow2: function (smovie) {
      this.selectedMovie = smovie
      this.dialog_detail = true
    },
    close2: function () {
      this.dialog_detail = false
    },
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