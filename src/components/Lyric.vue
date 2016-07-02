<template>
    <div class="lyric">
        <p></p>
        <P></P>
        <p></p>
    </div>
</template>
<script>
    export default {
        props: ['songData'],
        watch: {
            'songData': function (val, oldVal) {
//                this.$http({url: 'http://geci.me/api/lyric/' + val.song[0].title, method: 'get'}).then((response) => {
//                    console.log(JSON.parse(response.data))
//                    // success callback
//                }, (response) => {})
            }
        }
    }
</script>
<style>
   .lyric{
       display: flex;
       display: -webkit-flex;
       width: 375px;
       height: 80px;
   }
</style>