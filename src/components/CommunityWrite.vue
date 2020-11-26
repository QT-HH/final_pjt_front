<template>
<div>
  <v-card>
    <v-card-title>
      <span class="headline">게시글 작성</span>
    </v-card-title>
    <v-card-text>
      <v-container>
        <v-row>
          <v-col
            cols="12"
          >
            <v-text-field
              label="Review Title"
              required
              v-model="writeForm.title"
            ></v-text-field>
          </v-col>
          <v-col
            cols="12"
          >
          </v-col>
          <v-col
            cols="12"
          >
            <v-textarea
              label="Content"
              required
              v-model="writeForm.content"
            ></v-textarea>
          </v-col>
        </v-row>
      </v-container>
    </v-card-text>
    <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn
        color="blue darken-1"
        text
        @click="cancel"
      >
        Close
      </v-btn>
      <v-btn
        color="blue darken-1"
        text
        @click="write"
      >
        Save
      </v-btn>
    </v-card-actions>
  </v-card>
            
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
        this.$emit('getList')
        this.writeForm.title='',
        this.writeForm.movie_title='',
        this.writeForm.content='',
        this.writeForm.rank='',
        this.cancel()
      })
      .catch((err) => {
        console.log(newReview)
        console.error(err)
      })
      
    },
    cancel: function () {
      this.$emit('close')
    }
  }
}
</script>

<style>

</style>