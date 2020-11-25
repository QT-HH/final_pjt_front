<template>
  <v-app style="height:100%;">
    <v-card class="overflow-hidden">
      <div class="fixed-top">
        <v-app-bar
          absolute
          color="#6A76AB"
          dark
          src="https://picsum.photos/1920/1080?random"
        >

          <template v-slot:img="{ props }" class="d-block">
            <v-img
              v-bind="props"
              gradient="to top right, rgba(100,115,201,.7), rgba(25,32,72,.7)"
            ></v-img>
          </template>

          <v-toolbar-title> <h2>Title</h2> </v-toolbar-title>

          <v-spacer></v-spacer>

        
          <div v-if="login">
            <v-btn icon class="mx-2">
              <router-link @click.native="logout" to="#">Logout</router-link>
            </v-btn>
          </div>
          <div v-else> 
            <v-btn icon class="mx-2">
              <router-link :to="{ name: 'Signup' }">SignUp</router-link>
            </v-btn>

            <v-btn icon class="mx-2">
              <router-link :to="{ name: 'Login' }">Login</router-link>
            </v-btn>
          </div>

          <v-btn icon>
            <v-icon>mdi-magnify</v-icon>
          </v-btn>

          <v-btn icon>
            <v-icon>mdi-heart</v-icon>
          </v-btn>

          <template v-slot:extension>
            <v-tabs align-with-title>
              <v-tab><router-link :to="{ name: 'Home' }">Home</router-link></v-tab>
              <v-tab>Recommand</v-tab>
              <v-tab><router-link :to="{ name: 'Community' }">Community</router-link></v-tab>
            </v-tabs>
          </template>
        </v-app-bar>
      </div>
      <div style="height:120px;">

      </div>
      <v-sheet
        id="scrolling-techniques-3"
        class="overflow-y-auto"

      >
        <v-container>
          <router-view @login="login = true" />
        </v-container>
      </v-sheet>
    </v-card>
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
