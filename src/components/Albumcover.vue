<template>
  <div class="albumcover">
      <div class="album-wrap">
          <div class="row">
              <Search></Search>
              <a class="next-song" href="javascript:;" v-on:click="nextSong"></a>
          </div>
      </div>
      <img v-bind:src="albumsrc" width="375" height="375" class="albumimg" alt="">
  </div>
</template>

<script>
import Search from './Search.vue'
export default {
    props: ['song'],
    data () {
        return {
            albumsrc: ''
        }
    },
    methods: {
        nextSong () {
            this.$dispatch('get-song')
        }
    },
    components: {Search},
    watch: {
        'song': function (val, oldVal) {
            this.$set('albumsrc', val.song[0].picture)
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
    .album-wrap{
        position: absolute;
        width: 375px;
        height: 375px;
        padding: 30px;
        box-sizing: border-box;
    }
    .row{
        position: relative;
        width: 315px;
        height: 315px;
    }
    .next-song{
        display: flex;
        position: absolute;
        width: 20px;
        height: 22px;
        z-index: 9;
        right: 0;
        bottom: 0;
        align-items: flex-start;
        background: url(./img/nextSong.svg);
    }
</style>
