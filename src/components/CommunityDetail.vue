<template>

  <div
    class="mx-auto"
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

        <div class="grey--text ml-4">
          {{ selected.rank }}
        </div>
      </v-row>

      <div class="mt-4 subtitle-1">
        작성자 : {{ selected.user.username }}
      </div>
      <div class="subtitle-1">
        작성일 : {{ selected.created_at }}
      </div>
      <div class="subtitle-1">
        수정일 : {{ selected.updated_at }}
      </div>

      </v-card-text>
    <v-divider class="mx-4"></v-divider>

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
        @delCom="getComments"
        @putComment="getComments"
      />
    </v-card-text>

    <v-row>
      <v-col cols="10">
        <v-text-field
          label="comments"
          v-model="commentsInput"
          class="mx-4"
          @keydown.enter="createComment"
        ></v-text-field>
      </v-col>
      <v-col cols="2">
        <v-btn
          color="deep-purple lighten-2"
          text
          @click="createComment"
        >
          입력 
        </v-btn>

      </v-col>

    </v-row>

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
  </div>
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
    },
    delCom: function () {
      this.getComments()
    }
  },

  watch: {
    selected: function () {
      this.getComments()
      this.$emit('reviewDetail')
    }
  }
  
  
}
</script>

<style>

</style>