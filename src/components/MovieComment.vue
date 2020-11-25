<template>
  <div>
    {{comment.user.username}} ({{comment.rank}}) : {{comment.content}}
  </div>
</template>

<script>
export default {
  name:'MovieComment',
  props:{
    comment: Object,
  }
}
</script>

<style>

</style>