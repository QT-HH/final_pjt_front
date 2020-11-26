<template>
  <div>
    <h1>Signup</h1>
      <div>
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
        <v-text-field 
          label="Password Check"
          v-model="credentials.password2"
          :rules="rules"
          :type="'password'"
        ></v-text-field>

        <v-select
          :items="items"
          v-model="credentials.MBTI"
          item-text="state"
          item-value="abbr"
          label="MBTI"
        ></v-select>
      </div>

    <br>
          <v-btn
        color="primary"
        dark
        @click="signup"
      >
        회원가입
      </v-btn>

  </div>
</template>

<script>
import axios from 'axios'

const SERVER_URL = process.env.VUE_APP_SERVER_URL

export default {
  name: 'Signup',
  data: function () {
    return {
      credentials: {
        username: '',
        password: '',
        password2: '',
        MBTI:''
      },
      rules: [
        value => !!value || 'Required.',
        value => (value && value.length >= 3) || 'Min 3 characters',
      ],
      items: [
        { state: 'ESTJ', abbr: 1 },
        { state: 'ESTP', abbr: 2 },
        { state: 'ESFJ', abbr: 3 },
        { state: 'ESFP', abbr: 4 },
        { state: 'ENTJ', abbr: 5 },
        { state: 'ENTP', abbr: 6 },
        { state: 'ENFJ', abbr: 7 },
        { state: 'ENFP', abbr: 8 },
        { state: 'ISTJ', abbr: 9 },
        { state: 'ISTP', abbr: 10 },
        { state: 'ISFJ', abbr: 11 },
        { state: 'ISFP', abbr: 12 },
        { state: 'INTJ', abbr: 13 },
        { state: 'INFJ', abbr: 14 },
        { state: 'INFJ', abbr: 15 },
        { state: 'INFP', abbr: 16 },
      ],
    }
  },
  methods: {
    signup: function () {
      axios.post(`${SERVER_URL}/accounts/signup/`, this.credentials)
        .then((res) => {
          console.log(res)
          this.$router.push({ name: 'Login' })
        })
        .catch((err) => {
          console.log(err)
        })
    }
  }
}
</script>
