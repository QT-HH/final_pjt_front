<template>
  <div>
    <h2>리뷰 게시판</h2>
    <CommunityDetail v-if="detail_sign" :selected="selected" :index="index" @closeDetail="closeDetail"/>
    <v-btn @click="overlay = !overlay">
      Write
    </v-btn>
    <br>
    <v-overlay
      :absolute="absolute"
      :value="overlay"
    >
      <CommunityWrite />
        <v-btn
          color="orange lighten-2"
          @click="overlay = false"
        >
          Hide Overlay
        </v-btn>
    </v-overlay>
      <v-simple-table>
    <template v-slot:default>
      <thead>
        <tr>
          <th class="text-left">
            Title
          </th>
          <th class="text-left">
            MovieTitle
          </th>
          <th class="text-left">
            Rank
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(review,idx) in reviewList"
          :key="idx"
          :review="review"
          @click="reviewDetail(review,idx)"
        >
          <td>{{ review.title }}</td>
          <td>{{ review.movie_title }}</td>
          <td>{{ review.rank }}</td>
        </tr>
      </tbody>
    </template>
  </v-simple-table>

  </div>
</template>

<script>
import axios from 'axios'
import CommunityWrite from '../components/CommunityWrite.vue'
import CommunityDetail from '../components/CommunityDetail.vue'
const SERVER_URL = process.env.VUE_APP_SERVER_URL

export default {

  name:'community',
  components:{
    CommunityWrite,
    CommunityDetail,
  },
  data: function () {
    return {
      reviewList:[],
      absolute: true,
      opacity: 1,
      overlay: false,
      dialog: false,
      selected: {},
      index: 0,
      len: Number,
      detail_sign: false,
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
    reviewDetail: function (review,idx) {
      console.log('detail')
      this.selected = review
      this.index = this.len - idx
      this.detail_sign = true

      const config = this.setToken()

      axios.get(`${SERVER_URL}/communitys/${this.index}/detail/`, config)
        .then((res) => {
          console.log(res.data)
          this.selected = res.data
        })
        .catch((err) => {
          console.error(err)
        })
    },
    closeDetail: function () {
      this.selected = {}
      this.index = 0
      this.detail_sign = false
    }

  },
  created: function () {
    console.log(this.reviewList)
    const config = this.setToken()

    axios.get(`${SERVER_URL}/communitys/`, config)
      .then((res) => {
        console.log(res)
        this.reviewList = res.data
        this.len = this.reviewList.length
        console.log(this.len)
      })
      .catch((err) => {
        console.error(err)
      })
  },

}
</script>

<style>

</style>