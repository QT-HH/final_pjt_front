<template>
  <div class="home">
    <movies />
  </div>
</template>

<script>
// @ is an alias to /src
import movies from '../components/movies'

export default {
  name: 'Home',
  components: {
    movies,
  }
}
</script>
