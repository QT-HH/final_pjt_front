<template>

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

    <v-card-title>{{selected.title}}</v-card-title>

    <v-card-text>
      <v-row
        align="center"
        class="mx-0"
      >
        <v-rating
          :value=selected.rank
          color="amber"
          dense
          half-increments
          readonly
          size="14"
        ></v-rating>

        <div class="grey--text ml-4">
          {{ selected.rank }}
        </div>
      </v-row>

      <div class="mt-4 subtitle-1">
        {{ selected.user.username }}
      </div>

      </v-card-text>
    <v-divider class="mx-4"></v-divider>

    <v-card-title>{{selected.movie_title}}</v-card-title>

    <v-card-text>
      {{selected.content}}
    </v-card-text>

    <v-divider class="mx-4"></v-divider>


    <v-card-title>Comments</v-card-title>
    <v-card-text>
      <Comment 
        v-for="(comment,idx) in comments"
        :key="idx"
        :comment="comment"
      />
    </v-card-text>

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
      <v-btn
        color="deep-purple lighten-2"
        text
        @click="update(selected)"
      >
        수정
      </v-btn>
      <v-btn
        color="deep-purple lighten-2"
        text
        @click="del"
      >
        삭제
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
import axios from 'axios'
import Comment from '../components/Comment.vue'
const SERVER_URL = process.env.VUE_APP_SERVER_URL

export default {
  name:'CommunityDetail',
  data: function () {
    return {
      commentsInput: '',
      comments: '',
    }
  },
  props: {
    selected: Object,
    index: Number,
  },
  components:{
    Comment,
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
      this.$emit('closeDetail')
    },
    update: function (selected) {
      console.log('update')
      this.$emit('reviewUpdate',selected)
    },
    del: function () {
      this.$emit('delReview',this.selected.id)
    },
    getComments: function () {
      const config = this.setToken()

      axios.get(`${SERVER_URL}/communitys/${this.selected.id}/comments/`, config)
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
      }

      axios.post(`${SERVER_URL}/communitys/${this.selected.id}/comments/`, newComment, config)
        .then(() => {
          this.getComments()
          this.commentsInput= ''
        })
        .catch((err) => {
          console.error(err)
        })
    }
  },

  watch: {
    selected: function () {
      this.getComments()
    }
  }
  
  
}
</script>

<style>

</style>