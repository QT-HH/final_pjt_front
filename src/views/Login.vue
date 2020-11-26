<template>
    <div>
      <v-card 
        elevation="24"
        shaped
        class="col-3 col-md-2"
      >
      <h1>Login</h1>
        <div @keydown.enter="login">
          <v-text-field
            label="User name"
            hide-details="auto"
            v-model="credentials.username"
            :rules="rules"
          ></v-text-field>
          <v-text-field 
            label="Password"
            hide-details="auto"
            v-model="credentials.password"
            :rules="rules"
            :type="'password'"
          ></v-text-field>

        </div>

      <br>
      <v-btn
        color="primary"
        dark
        @click="login"
      >
        로그인
      </v-btn>
      <br>
      <br>
      만약 계정이 없다면? <a href='' @click="gotoSignup"> SignUp </a>
      </v-card>
  </div>
</template>

<script>
import axios from 'axios'

const SERVER_URL = process.env.VUE_APP_SERVER_URL

export default {
  name: 'Login',
  data: function () {
    return {
      credentials: {
        username: '',
        password: '',
      }
    }
  },
  methods: {
    login: function () {
      axios.post(`${SERVER_URL}/accounts/api-token-auth/`, this.credentials)
        .then((res) => {
          // console.log(res)
          localStorage.setItem('jwt', res.data.token)
          this.$emit('login')
          this.$router.push({ name: 'Home' })
        })
        .catch((err) => {
          console.log(err)
        })
    },
    gotoSignup: function () {
      this.$router.push({ name: 'Signup' })
    }
  }
}
</script>
