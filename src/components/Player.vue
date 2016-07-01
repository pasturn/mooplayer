<template>
    <div class="palyer">
        <Song :song-info="song"></Song>
        <Lyric></Lyric>
        <Control :song-data="song"></Control>
    </div>
</template>
<script>
import Song from './Song.vue'
import Lyric from './Lyric.vue'
import Control from './Controler.vue'
export default {
    props: ['song'],
    data () {
        return {
            data: {}
        }
    },
    components: {
        Song,
        Lyric,
        Control
    }
}
</script>
<style scoped>
   .palyer{
       width: 375px;
       height: 292px;
   }
</style>