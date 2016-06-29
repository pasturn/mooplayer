<template>
    <div class="palyer">
        <Song></Song>
        <Lyric></Lyric>
        <Control></Control>
    </div>
</template>
<script>
    import Song from './Song.vue'
    import Lyric from './Lyric.vue'
    import Control from './Controler.vue'
    export default {
        components: {Song, Lyric, Control}
    }
</script>
<style>
   .palyer{
       width: 375px;
       height: 292px;
   }
</style>