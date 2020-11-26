<template>
  <div class="text-center">
    <div>
      <v-hover>
          <template v-slot:default="{ hover }">
            <v-card
              class="mx-auto text-center"
              max-width="380"
            >
              <v-img :src=imgSrc style="width:400px; height:560px;" class="mx-auto"></v-img>

              <v-fade-transition>
                <v-overlay
                  v-if="hover"
                  absolute
                  color="#036358"
                >
                  <v-btn @click="detailShow(movie)">{{movie.title}}</v-btn>
                </v-overlay>
              </v-fade-transition>
            </v-card>
          </template>
        </v-hover>
    </div>
  </div>

  

</template>

<script>

export default {
  name:'movie',
  props: {
    movie: Object,
  },
  computed: {
    imgSrc: function () {
      return `https://image.tmdb.org/t/p/w500${this.movie.poster_path}`
    }
  },
  methods: {
    detailShow: function (movie) {
      this.$emit('detailShow4',movie)
    },
  },
}
</script>

<style>

</style>