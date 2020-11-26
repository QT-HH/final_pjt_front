<template>
  <!-- <div>
    {{comment.user.username}} : {{comment.content}}
    
  </div> -->
  <div class="row">
    <div class="col-6" @click="putSignal">
      {{comment.user.username}} : {{comment.content}}
    </div>
    <div class="col-6">
      at {{comment.created_at}}
      <button @click="delComment"> X </button>
    </div>
    <v-row v-if="putSig">
      <v-col cols="10">
        <v-text-field
          label="comments"
          v-model="commentsInput"
          class="mx-4"
          @keydown.enter="putComment"
        ></v-text-field>
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
  name:'Comment',
  props:{
    comment: Object,
  },
  data: function () {
    return {
      putSig: false,
      commentsInput: this.comment.content,
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

      axios.delete(`${SERVER_URL}/communitys/comments/${this.comment.id}/`, config)
        .then((res) => {
          this.comments = res.data
          this.$emit('delCom')
        })
        .catch((err) => {
          console.error(err)
        })
    },
    putSignal: function () {
      console.log(this.putSig)
      this.putSig = !this.putSig
    },
    putComment: function () {
      const config = this.setToken()

      const newContent = {
        content: this.commentsInput
      }

      axios.put(`${SERVER_URL}/communitys/comments/${this.comment.id}/`, newContent, config)
        .then(() => {
          this.putSig = false
          this.$emit('putComment')
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