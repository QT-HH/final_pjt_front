<template>
  <v-app style="background-color:#D8D8D8;">
    <div>
      <div class="fixed-top">
        <v-app-bar
          dark
          class="bg-info"
        >

          <template v-slot:img="{ props }" class="d-block">
            <v-img
              v-bind="props"
              gradient="to top right, rgba(100,115,201,.7), rgba(25,32,72,.7)"
            ></v-img>
          </template>

          <!-- <v-toolbar-title> <img alt="Vue logo" src="./assets/100j.png"> </v-toolbar-title> -->
          <router-link :to="{ name: 'Home' }" class="pt-4 pl-3">
            <img alt="Vue logo" src="./assets/100jj.png" style="width:60px; height:60px;">
          </router-link>
          <!-- <v-toolbar-title> <h2>100J</h2> </v-toolbar-title> -->

          <v-spacer></v-spacer>

        
          <div v-if="login">
            <v-btn icon class="mx-2">
              <router-link @click.native="logout" to="#" class="text-white">Logout</router-link>
            </v-btn>
          </div>
          <div v-else> 
            <v-btn icon class="mx-2">
              <router-link :to="{ name: 'Signup' }" class="text-white"> SignUp</router-link>
            </v-btn>

            <v-btn icon class="mx-2">
              <router-link :to="{ name: 'Login' }" class="text-white">Login</router-link>
            </v-btn>
          </div>

          <!-- <v-btn icon>
            <v-icon>mdi-magnify</v-icon>
          </v-btn>

          <v-btn icon>
            <v-icon>mdi-heart</v-icon>
          </v-btn> -->

          <template v-slot:extension>
            <v-tabs align-with-title>
              <v-tab><router-link :to="{ name: 'Home' }" class="text-white">Home</router-link></v-tab>
              <v-tab><router-link :to="{ name: 'Recommand' }" class="text-white">Recommand</router-link></v-tab>
              <v-tab><router-link :to="{ name: 'Community' }" class="text-white">Community</router-link></v-tab>
            </v-tabs>
          </template>
        </v-app-bar>
      </div>
      <div style="height:120px; background-color:#D8D8D8;">

      </div>
      <v-sheet
        id="scrolling-techniques-3"
        class="overflow-y-auto"
        style="background-color:#D8D8D8;"

      >
        <div class="container">
          <router-view @login="login = true" />
        </div>
      </v-sheet>
    </div>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  components: {
  },

  data: function () {
    return {
      login: false,
    }
  },
  methods: {
    logout: function () {
      localStorage.removeItem('jwt')
      this.login = false
      this.$router.push({ name: 'Login' })
    }
  },
  created: function () {
    const token = localStorage.getItem('jwt')

    if (token) {
      this.login = true
    }
  }
};
</script>
