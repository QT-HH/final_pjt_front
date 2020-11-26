<template>
  <div class="container">
    <div v-if="login">
      <h2>영화 추천</h2>
        <div class="row text-center justify-content-around">
          
          <v-card 
            elevation="24"
            shaped
            class="col-3 col-md-2"
          >
          <div class="row">
            <div class="col-12">
              <h4>{{this.MBTI}}를 위한 영화</h4>
            </div>
            <div class="col-12">
              <movie4 :movie="movie4" @detailShow4="detailShow" />
            </div>
            <div class="col-12">
              <button @click="getMovie4" class="m-3 pb-1 btn btn-info"> <h3> 다른 영화 </h3> </button>

            </div>
          </div>
          </v-card>

          <v-card 
            elevation="24"
            shaped
            class="col-3 col-md-2"
          >
          <div v-if="movie2" class="row">
            <div class="col-12">
              <h4>{{this.movieRec.title}} 와 비슷한 영화</h4>
            </div>
            <div class="col-12">
              <movie2 :movie="movie2" @detailShow2="detailShow" />
            </div>
            <div class="col-12">
              <button @click="getMovie2" class="m-3 pb-1 btn btn-info"> <h3> 다른 영화 </h3> </button>
            </div>
          </div>
          <div v-else>
            먼저 시청한 영화의 평점을 등록해주세요.
          </div>
          </v-card>

          <v-card 
            elevation="24"
            shaped
            class="col-3 col-md-2"
          >
          <div class="row">
            <div class="col-12">
              <h4>지금 <span class="font-weight-bolder">{{movie3.title}}</span>이(가) 땡겨요!</h4>
            </div>
            <div class="col-12">
              <movie3 :movie="movie3" @detailShow3="detailShow" />
            </div>
            <div class="col-12">
              <button @click="getMovie3" class="m-3 pb-1 btn btn-info"> <h3> 다른 영화 </h3> </button>
            </div>
          </div>
          </v-card>
 
        </div>
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
    <div v-else>
      <h1> 먼저 <a href="" @click="gotoLogin">로그인</a>을 해주세요. </h1>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import movie4 from '../components/movie4'
import movie2 from '../components/movie2'
import movie3 from '../components/movie3'
import MovieDetail from '../components/MovieDetail'
const SERVER_URL = process.env.VUE_APP_SERVER_URL

export default {
  name: 'movies',
  components: {
    movie4,
    movie2,
    movie3,
    MovieDetail,
  },
  data: function () {
    return {
      movie4: {},
      movie2: {},
      movie3: {},
      movieRec: {},
      selectedMovie: {},
      dialog_detail: false,
      login:false,
      mbti_genres: {
        1: 'ESTJ',
        2: 'ESTP',
        3: 'ESFJ',
        4: 'ESFP',
        5: 'ENTJ',
        6: 'ENTP',
        7: 'ENFJ',
        8: 'ENFP',
        9: 'ISTJ',
        10: 'ISTP',
        11: 'ISFJ',
        12: 'ISFP',
        13: 'INTJ',
        14: 'INFJ',
        15: 'INFJ',
        16: 'INFP',
      },
      MBTI:'',
    }
  },
  created: function () {
    const token = localStorage.getItem('jwt')

    if (token) {
      this.login = true
      this.getuser()
    
    this.getMovie4()
    this.getMovie2()
    this.getMovie3()
    this.getMovieComment()
    } else {
      this.$router.push({ name: 'Login' })
    }
  },
  methods: {
    setToken: function () {
      const token = localStorage.getItem('jwt')

      const config = {
        headers: {
          Authorization: `JWT ${token}`
        }
      }
      return config
    },
    getMovie4: function () {
      const config = this.setToken()

      axios.get(`${SERVER_URL}/movies/recommanded_1/`,config)
        .then((res) => {
          this.movie4 = res.data
          // console.log(this.movie)
        })
        .catch((err) => {
          console.error(err)
        })
    },
    getMovie2: function () {
      const config = this.setToken()

      axios.get(`${SERVER_URL}/movies/recommanded_2/`,config)
        .then((res) => {
          if (res.data.detail === false) {
            this.movie2 = res.data.detail
          } else {
            this.movie2 = res.data
          }
          // console.log(this.movie)
        })
        .catch((err) => {
          console.error(err)
        })
    },
    getMovie3: function () {
      const config = this.setToken()

      axios.get(`${SERVER_URL}/movies/recommanded_3/`,config)
        .then((res) => {
          this.movie3 = res.data
          // console.log(this.movie)
        })
        .catch((err) => {
          console.error(err)
        })
    },
    getMovieComment: function () {
      const config = this.setToken()

      axios.get(`${SERVER_URL}/movies/recommanded_comment/`,config)
        .then((res) => {
          this.movieRec = res.data
          // console.log(this.movie)
        })
        .catch((err) => {
          console.error(err)
        })
    },
    detailShow: function (smovie) {
      this.selectedMovie = smovie
      this.dialog_detail = true
    },
    close: function () {
      this.dialog_detail = false
    },
    gotoLogin: function () {
      this.$router.push({ name: 'Login' })
    },
    getuser: function () {
      const config = this.setToken()

      axios.get(`${SERVER_URL}/accounts/getuser/`,config)
        .then((res) => {
          // console.log(res)
          this.MBTI=this.mbti_genres[res.data[0]]
        })
        .catch((err) => {
          console.error(err)
        })
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