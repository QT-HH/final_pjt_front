<template>
  <div>
    <v-card
      class="mx-auto my-12"
      max-width="748"
    >
      <template slot="progress">
        <v-progress-linear
          color="deep-purple"
          height="10"
          indeterminate
        ></v-progress-linear>
      </template>

      <v-card-title>{{movie.title}}</v-card-title>

      <v-divider class="mx-4"></v-divider>

      <v-card-text>
        {{movie.overview}}
      </v-card-text>

      <v-divider class="mx-4"></v-divider>


      <v-card-title>Comments</v-card-title>

      <v-card-text>
        <MovieComment 
          v-for="(comment,idx) in comments"
          :key="idx"
          :comment="comment"
        />
      </v-card-text>

      <v-select
        :items="[1,2,3,4,5]"
        label="rank"
        v-model="rank"
        required
        class="mx-4"
      ></v-select>

      <v-text-field
        label="comments"
        v-model="commentsInput"
        class="mx-4"
      ></v-text-field>
      <v-btn
        color="deep-purple lighten-2"
        text
        @click="createComment"
      >
        입력
      </v-btn>

      <v-card-actions>
        <v-btn
          color="deep-purple lighten-2"
          text
          @click="close"
        >
          닫기
        </v-btn>

      </v-card-actions>
    </v-card>
  </div>
</template>

<script>
import axios from 'axios'
import MovieComment from '../components/MovieComment.vue'
const SERVER_URL = process.env.VUE_APP_SERVER_URL

export default {
  name:'MovieDetail',
  components:{
    MovieComment,
  },
  data: function () {
    return {
      rank: Number,
      commentsInput: '',
      comments: [],
    }
  },
  props:{
    movie:Object
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
    close: function () {
      this.$emit('close')
    },
    getComments: function () {
      const config = this.setToken()

      axios.get(`${SERVER_URL}/movies/${this.movie.id}/comments/`, config)
        .then((res) => {
          this.comments = res.data
        })
        .catch((err) => {
          console.error(err)
        })
    },
    createComment: function () {
      const config = this.setToken()

      const newComment = {
        content: this.commentsInput,
        rank: this.rank,
      }

      axios.post(`${SERVER_URL}/movies/${this.movie.id}/comments/`, newComment, config)
        .then(() => {
          this.getComments()
          this.commentsInput= ''
        })
        .catch((err) => {
          console.error(err)
        })
    }
  },
  created: function () {
    this.getComments()
  },
  watch: {
    movie: function () {
      this.getComments()
    }
  }
}
</script>

<style>

</style>