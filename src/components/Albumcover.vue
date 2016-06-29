<template>
  <div class="albumcover">
      <Search></Search>
      <img v-bind:src="albumsrc" width="375" height="375" class="albumimg" alt="">
  </div>
</template>

<script>
import Search from './Search.vue'
export default {
    components: {Search},
    data () {
        return {
          // note: changing this line won't causes changes
          // with hot-reload because the reloaded component
          // preserves its current state and we are modifying
          // its initial state.
            albumsrc: 'https://img1.doubanio.com/lpic/s1482698.jpg'
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .albumcover{
        width: 375px;
        height: 375px;
    }
    .albumimg{
        position: relative;
        z-index: 1;
    }
</style>
