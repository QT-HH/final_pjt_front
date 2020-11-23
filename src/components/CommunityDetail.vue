<template>
  <v-card
    v-if="selected"
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
        {{ selected.user }}
      </div>

      </v-card-text>
    <v-divider class="mx-4"></v-divider>

    <v-card-title>{{selected.movie_title}}</v-card-title>

    <v-card-text>
      {{selected.content}}
    </v-card-text>

    <v-card-actions>
      <v-btn
        color="deep-purple lighten-2"
        text
        @click="close"
      >
        닫기
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>

export default {
  name:'CommunityDetail',
  props: {
    selected: Object,
    index: Number,
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
    }
  },
  created: function () {
    console.log(this.selected)
  },
}
</script>

<style>

</style>