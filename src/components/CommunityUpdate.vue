<template>
<div>
  <v-card>
    <v-card-title>
      <span class="headline">리뷰 수정</span>
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
  name:'CommunityUpdate',
  data: function () {
    return {
      writeForm: {
        title: this.sel.title,
        movie_title: this.sel.movie_title,
        content: this.sel.content,
        rank: this.sel.rank,
      },
      items: [
        1,2,3,4,5
      ]
    }
  },
  props:{
    sel: Object,
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
      console.log('update')
      
      const config = this.setToken()

      const newReview = {
        title: this.writeForm.title,
        movie_title: this.writeForm.movie_title,
        content: this.writeForm.content,
        rank: this.writeForm.rank,
      }
      axios.put(`${SERVER_URL}/communitys/${this.sel.id}/`,newReview, config)
      .then(() => {
        this.sel.title = this.writeForm.title
        this.sel.movie_title = this.writeForm.movie_title
        this.sel.content = this.writeForm.content
        this.sel.rank = this.writeForm.rank
        this.$emit('getList')
        this.cancel()
      })
      .catch((err) => {
        console.error(err)
      })
      
    },
    cancel: function () {
      this.$emit('closeUpdate')
    }
  }
}
</script>

<style>

</style>