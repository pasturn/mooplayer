<template>
  <div id="app">
    <Albumcover :song="data" v-on:get-song="getSong"></Albumcover>
    <Player :song="data" v-on:get-song="getSong"></Player>
  </div>
</template>
<script>
import Albumcover from './components/Albumcover.vue'
import Player from './components/Player.vue'
export default {
    ready () {
        this.getSong()
    },
    data () {
        return {
            data: {}
        }
    },
    methods: {
        getSong () {
            this.$http({url: 'http://api.jirengu.com/fm/getSong.php', method: 'get'}).then((response) => {
                console.log(JSON.parse(response.data))
                this.$set('data', JSON.parse(response.data))
                // success callback
            }, (response) => {}
        ) }
    },
    components: {Albumcover, Player}
}
</script>
<style>
html {
    height: 100%;
}
body {
    display: flex;
    display: -webkit-flex;
    align-items: center;
    justify-content: center;
    height: 100%;
    font-size: 14px;
    font-family: Source Sans Pro, Helvetica, sans-serif;
    background: #e2e2e2;
}
#app {
    background: #ffffff;
    max-width: 600px;
    width:375px;
    height: 667px;
    box-shadow: 0 2px 3px #999999;
}
</style>
