<template>
  <Post :data="data[0]" />
  <Post :data="data[1]" />
  <Post :data="data[2]" />
</template>

<script>
import Post from './Post.vue'
export default {
  components: {
    Post
  },
  props: {
    data : Array
  }
}
</script>

<style>

</style>