<template>
  <!-- <div class="row">
    <div class="col-6">
      {{comment.user.username}} ({{comment.rank}}) : {{comment.content}}
    </div>
    <div class="col-6">
      {{comment.created_at}}
    </div>
  </div> -->
  <div class="row">
    <div class="col-6" @click="putSignal">
      {{comment.user.username}} rank({{comment.rank}}): {{comment.content}}
    </div>
    <div class="col-6">
      at {{comment.created_at}}
      <button @click="delComment"> X </button>
    </div>
    <v-row v-if="putSig">
      <v-col cols="6">
        <v-text-field
          label="review input"
          v-model="commentsInput"
          class="mx-4"
        ></v-text-field>
      </v-col>
      <v-col cols="4">
        <v-select
          :items="[1,2,3,4,5]"
          label="rank"
          v-model="rank"
          required
          class="mx-4"
        ></v-select>
      </v-col>
      <v-col cols="2">
        <v-btn
          color="deep-purple lighten-2"
          text
          @click="putComment"
        >
          입력 
        </v-btn>

      </v-col>

    </v-row>
  </div>
</template>

<script>
import axios from 'axios'
const SERVER_URL = process.env.VUE_APP_SERVER_URL

export default {
  name:'MovieComment',
  props:{
    comment: Object,
  },
  data: function () {
    return {
      putSig: false,
      commentsInput: this.comment.content,
      rank: this.comment.rank
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

    delComment: function () {
      const config = this.setToken()

      axios.delete(`${SERVER_URL}/movies/comments/${this.comment.id}/`, config)
        .then((res) => {
          this.comments = res.data
          this.$emit('delputComm')
        })
        .catch((err) => {
          console.error(err)
        })
    },
    putSignal: function () {
      this.putSig = !this.putSig
    },
    putComment: function () {
      const config = this.setToken()

      const newContent = {
        content: this.commentsInput,
        rank: this.rank
      }

      axios.put(`${SERVER_URL}/movies/comments/${this.comment.id}/`, newContent, config)
        .then(() => {
          this.putSig = false
          this.$emit('delputComm')
        })
        .catch((err) => {
          console.error(err)
        })
    },
  }

}
</script>

<style>

</style>