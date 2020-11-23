<template>
  <div>
    <h1>리뷰 작성</h1>
      <div>
        <v-text-field
          label="Review Title"
          hide-details="auto"
          v-model="writeForm.title"
          :rules="rules"
        ></v-text-field>
        <v-text-field 
          label="Movie Title"
          hide-details="auto"
          v-model="writeForm.movie_title"
          :rules="rules"
        ></v-text-field>
        <v-textarea
          label="Content"
          hide-details="auto"
          v-model="writeForm.content"
          :rules="rules"
        ></v-textarea>
        <v-select
          :items="items"
          v-model="writeForm.rank"
          label="ratings"
        ></v-select>
      </div>

    <br>
    <button @click="write" >작성</button>

  </div>
</template>

<script>
import axios from 'axios'
const SERVER_URL = process.env.VUE_APP_SERVER_URL

export default {
  name:'CommunityWrite',
  data: function () {
    return {
      writeForm: {
        title: '',
        movie_title: '',
        content:'',
        rank: '',
      },
      rules: [
        value => !!value || 'Required.',
        value => (value && value.length >= 3) || 'Min 3 characters',
      ],
      items: [
        1,2,3,4,5
      ]
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
    write: function () {
      console.log('write')

      const config = this.setToken()

      const newReview = {
        title: this.writeForm.title,
        movie_title: this.writeForm.movie_title,
        content: this.writeForm.content,
        rank: this.writeForm.rank,
      }
      axios.post(`${SERVER_URL}/communitys/`,newReview, config)
      .then(() => {
        console.log(newReview)
      })
      .catch((err) => {
        console.log(newReview)
        console.error(err)
      })
    },
  }
}
</script>

<style>

</style>