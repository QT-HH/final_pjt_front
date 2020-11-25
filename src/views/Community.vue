<template>
  <div>
    <h2>리뷰 게시판</h2>
    <CommunityDetail v-if="detail_sign" :selected="selected" :index="index" @closeDetail="closeDetail" @delReview="delReview"  @reviewUpdate="reviewUpdate" />
      
    <v-row>
      <v-dialog
        v-model="dialog_write"
        persistent
        max-width="600px"
      >
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            color="primary"
            dark
            v-bind="attrs"
            v-on="on"
          >
            리뷰 작성
          </v-btn>
        </template>
        <CommunityWrite @close="dialog_write = false" @getList="getList" />
      </v-dialog>
    </v-row>

    <v-row justify="center">
      <v-dialog
        v-model="dialog_update"
        persistent
        max-width="600px"
      >
        <template>
        </template>
        <CommunityUpdate @closeUpdate="dialog_update = false" @getList="getList" :sel="selected"/>
      </v-dialog>
    </v-row>

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
            <th class="text-left">
              Writer
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(review,idx) in reviewList"
            :key="idx"
            :review="review"
            @click="reviewDetail(review)"
          >
            <td>{{ review.title }}</td>
            <td>{{ review.movie_title }}</td>
            <td>{{ review.rank }}</td>
            <td>{{ review.user.username }}</td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>

  </div>
</template>

<script>
import axios from 'axios'
import CommunityWrite from '../components/CommunityWrite.vue'
import CommunityUpdate from '../components/CommunityUpdate.vue'
import CommunityDetail from '../components/CommunityDetail.vue'
const SERVER_URL = process.env.VUE_APP_SERVER_URL

export default {

  name:'community',
  components:{
    CommunityWrite,
    CommunityUpdate,
    CommunityDetail,
  },
  data: function () {
    return {
      reviewList:[],
      absolute: true,
      opacity: 1,
      overlay: false,
      dialog_write: false,
      dialog_update: false,
      selected: {},
      index: 0,
      len: Number,
      detail_sign: false,
      del_id:Number,
    }
  },
  methods: {
    getList: function () {
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
    setToken: function () {
      const token = localStorage.getItem('jwt')

      const config = {
        headers: {
          Authorization: `JWT ${token}`
        }
      }
      return config
    },
    reviewDetail: function (review) {
      console.log('detail')
      this.selected = review
      this.detail_sign = true

      const config = this.setToken()

      axios.get(`${SERVER_URL}/communitys/${this.selected.id}/detail/`, config)
        .then((res) => {
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
    },
    delReview: function (delidx) {
      console.log('dell')
      const token = localStorage.getItem('jwt')

      const config = {
        headers: {
          Authorization: `JWT ${token}`
        }
      }
      axios.delete(`${SERVER_URL}/communitys/${delidx}/`, config)
        .then((res) => {
          console.log(res)
          this.detail_sign=false
          this.getList()
        })
        .catch((err) => {
          console.log(err)
        })
    },
    reviewUpdate: function (sel) {
      this.selected = sel
      this.dialog_update=true
    },

  },
  created: function () {
    this.getList()
  },

}
</script>

<style>

</style>