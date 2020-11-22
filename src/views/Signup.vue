<template>
  <div>
    <h1>Signup</h1>
    <div>
      <label for="username">사용자 이름: </label>
      <input type="text" id="username" v-model="credentials.username">
    </div>
    <div>
      <label for="password">비밀번호: </label>
      <input type="password" id="password" v-model="credentials.password">
    </div>
    <div>
      <label for="password2">비밀번호 확인: </label>
      <input 
        type="password" 
        id="password2" 
        v-model="credentials.password2"
        @keypress.enter="signup"
      >
    </div>

    <label for="choice">당신의 MBTI는?</label>
    <select name="choice" id="choice" v-model="credentials.MBTI">
      <option value=1>ESTJ</option>
      <option value=2>ESTP</option>
      <option value=3>ESFJ</option>
      <option value=4>ESFP</option>
      <option value=5>ENTJ</option>
      <option value=6>ENTP</option>
      <option value=7>ENFJ</option>
      <option value=8>ENFP</option>
      <option value=9>ISTJ</option>
      <option value=10>ISTP</option>
      <option value=11>ISFJ</option>
      <option value=12>ISFP</option>
      <option value=13>INTJ</option>
      <option value=14>INFJ</option>
      <option value=15>INFJ</option>
      <option value=16>INFP</option>
    </select>
    <br>
    <button @click="signup">회원가입</button>

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
        MBTI:'',
      }
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
