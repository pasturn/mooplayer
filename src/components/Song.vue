<template>
    <div class="song">
        <span class="song-title" v-html="title"></span>
        <span class="song-author" v-html="artist"></span>
    </div>
</template>
<script>
    export default {
        props: ['songInfo'],
        data () {
            return {
                title: '',
                artist: ''
            }
        },
        watch: {
            'songInfo': function (val, oldVal) {
                this.$set('title', val.song[0].title)
                this.$set('artist', val.song[0].artist)
            }
        }
    }
</script>
<style>
   .song{
       display: flex;
       display: -webkit-flex;
       flex-direction:row;
       justify-content: space-between;
       width: 375px;
       box-sizing: border-box;
       padding: 15px 30px;

   }
    .song-title{
        font-size: 1em;
        color: #333333;
    }
    .song-author{
        font-size: 1em;
        color: #999999;
    }
</style>