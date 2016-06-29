<template>
    <div class="bar">
        <div class="timer">
            <span>{{currentTime}}/{{duration}}</span>
        </div>
        <div class="progress-wrap">
            <div class="point" :style="{ left: activeWidth + '%' }"></div>
            <div class="progress"></div>
            <div class="active-progress" :style="{ width: activeWidth + '%' }"></div>
        </div>

    </div>
    <div class="controler">
        <div class="side">
            <a class="loop" href="javascript:;">循环</a>
        </div>
        <div class="s-warp">
            <a class="s-run btn-previous" href="javascript:;" v-on:click="actionPrevious"></a>
        </div>
        <div class="b-warp">
            <a class="b-run" v-bind:class="{ 'btn-play': isPause, 'btn-pause': isPlay }" href="javascript:;" v-on:click="actionPlay"></a>
        </div>
        <div class="s-warp">
            <a class="s-run btn-next" href="javascript:;" v-on:click="actionNext"></a>
        </div>
        <audio v-el:audio
               @timeupdate="onTimeUpdateListener"
               @play="isPlayListener"
               @pause="isPlayListener"
               @ended="isPlayListener"
               id="jp_audio_0"
               preload="metadata"
               src="http://rm.sina.com.cn/wm/VZ2010050511043310440VK/music/MUSIC1005051622027270.mp3"></audio>
    </div>
</template>
<script>
    export default {
        data () {
            return {
                activeWidth: 0,
                currentTime: '00:00', // The initial current time
                duration: '00:00',
                isPause: true,
                isPlay: false
            }
        },
        methods: {
            formatTime (time) {
                var _minutes = time.getMinutes() >= 10 ? time.getMinutes() : '0' + time.getMinutes()
                var _seconds = time.getSeconds() >= 10 ? time.getSeconds() : '0' + time.getSeconds()
                return (_minutes + ':' + _seconds)
            },
            actionPlay () {
                if (this.$els.audio.readyState === 4) {
                    var _duration = new Date(this.$els.audio.duration * 1000)
                    this.duration = this.formatTime(_duration)
                    if (this.$els.audio.paused) {
                        this.$els.audio.play()
                    } else {
                        this.$els.audio.pause()
                    }
                }
            },
            actionPrevious () {
                this.$els.audio.currentTime -= 5
            },
            actionNext () {
                this.$els.audio.currentTime += 5
            },
            isPlayListener () {
                if (this.$els.audio.paused || this.$els.audio.ended) {
                    this.isPlay = false
                    this.isPause = true
                } else {
                    this.isPlay = true
                    this.isPause = false
                }
            },
            onTimeUpdateListener () {
                // Update current time
                var _currentTime = new Date(this.$els.audio.currentTime * 1000)
                this.currentTime = this.formatTime(_currentTime)
                this.activeWidth = (this.$els.audio.currentTime / this.$els.audio.duration) * 100
                if (this.$els.audio.ended === true) {
                    this.isPlay = false
                    this.isPause = true
                }
            }
        }
    }
</script>
<style>
    .bar{
        display: flex;
        display: -webkit-flex;
        flex-direction:column;
        justify-content: center;
        width: 375px;
        box-sizing: border-box;
        padding: 0 30px;
    }
    .timer{
        align-self: flex-end;
        color: #999999;
        padding: 7px 0;

    }
    .progress-wrap{
        width: 315px;
        height: 6px;
        position: relative;
    }
    .point{
        height: 12px;
        background: #fff;
        width: 12px;
        position: absolute;
        top: -3px;
        margin-left: -6px;
        border-radius:8px;
        box-shadow: 0 1px 2px #999999;
        z-index: 99;
    }
    .progress{
        width: 315px;
        height: 6px;
        background: #f5f5f5;
        box-shadow: 0 1px 2px #dddddd inset;
        border-radius: 5px;
        position: absolute;
        z-index: 1;
    }
    .active-progress{
        height: 6px;
        position: absolute;
        background: #3EDCFE;
        box-shadow: 0 1px 1px rgba(0, 0, 0, 0.31) inset;
        border-radius: 5px;
        z-index: 3;
    }
    .controler{
        display: flex;
        display: -webkit-flex;
        flex-direction:row;
        justify-content: center;
        align-items: center;
        width: 375px;
        box-sizing: border-box;
        padding: 20px 30px;
    }
    .b-warp {
        display: flex;
        display: -webkit-flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        width: 85px;
        height: 85px;
        border-radius: 50px;
        background: -webkit-linear-gradient(#f5f5f5, #f8f8f8); /* Safari 5.1 - 6.0 */
        background: -o-linear-gradient(#f5f5f5, #f8f8f8); /* Opera 11.1 - 12.0 */
        background: -moz-linear-gradient(#f5f5f5, #f8f8f8); /* Firefox 3.6 - 15 */
        background: linear-gradient(#f5f5f5, #f8f8f8); /* 标准的语法 */

    }
    .s-warp{
        display: flex;
        display: -webkit-flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        width: 45px;
        height: 45px;
        border-radius:50px;
        background: -webkit-linear-gradient(#f5f5f5, #f8f8f8); /* Safari 5.1 - 6.0 */
        background: -o-linear-gradient(#f5f5f5, #f8f8f8); /* Opera 11.1 - 12.0 */
        background: -moz-linear-gradient(#f5f5f5, #f8f8f8); /* Firefox 3.6 - 15 */
        background: linear-gradient(#f5f5f5, #f8f8f8); /* 标准的语法 */
    }
    .b-run{
        display: flex;
        display: -webkit-flex;
        justify-content: center;
        align-items: center;
        border-radius: 50px;
        width: 71px;
        height: 71px;
        box-shadow: 0 1px 2px #999999;
    }
    .s-run{
        display: flex;
        display: -webkit-flex;
        justify-content: center;
        align-items: center;
        border-radius: 50px;
        width: 33px;
        height: 33px;
        box-shadow: 0 1px 2px #999999;
        }
    .b-run:active{
        box-shadow: 0 1px 1px #bbbbbb;
    }
    .s-run:active{
        box-shadow: 0 1px 1px #bbbbbb;
    }
    .btn-play{
        background: url(./img/play_normal.svg) #ffffff;
        /*transition:background-image 1.5s;*/
        /*-moz-transition:background-image 1.5s; /!* Firefox 4 *!/*/
        /*-webkit-transition:background-image 1.5s; /!* Safari and Chrome *!/*/
        /*-o-transition:background-image 1.5s; /!* Opera *!/*/
    }
    .btn-play:hover{
        background-image: url(./img/play_active.svg);
    }
    .btn-pause{
        background: url(./img/pause_normal.svg) #ffffff;
        /*transition:background-image 1.5s;*/
        /*-moz-transition:background-image 1.5s; /!* Firefox 4 *!/*/
        /*-webkit-transition:background-image 1.5s; /!* Safari and Chrome *!/*/
        /*-o-transition:background-image 1.5s; /!* Opera *!/*/
    }
    .btn-pause:hover{
        background-image: url(./img/pause_active.svg);
    }
    .btn-previous{
        background: url(./img/previous_normal.svg) #ffffff;
    }
    .btn-previous:hover{
        background: url(./img/previous_active.svg) #ffffff;
    }
    .btn-next{
        background: url(./img/next_normal.svg) #ffffff;
    }
    .btn-next:hover{
        background: url(./img/next_active.svg) #ffffff;
    }
    .side{

    }
</style>