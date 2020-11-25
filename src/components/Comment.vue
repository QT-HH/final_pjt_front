<template>
  <div>
    {{comment.user.username}} : {{comment.content}}
    <button @click="delComment"> x </button>
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