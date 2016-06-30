<template>
    <div class="bar">
        <div class="timer">
            <span>{{currentTime}}/{{duration}}</span>
        </div>
        <div class="progress-wrap" v-on:click.stop="setPointPosition">
            <div class="point" v-on:mousedown="setMouseDrag"  :style="{ left: activePosition + '%' }"></div>
            <div class="active-progress" :style="{ width: activePosition + '%' }"></div>
            <div class="buffered-progress" :style="{ width: bufferedPosition + '%' }"></div>
            <div class="progress" ></div>
        </div>
    </div>
    <div class="controler">
        <div class="left-side">
            <a class="loop" v-bind:class="{ 'single-loop': isSingle, 'all-loop': !isSingle }" v-on:click="singleLoop" href="javascript:;" ></a>
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
        <div class="right-side">
            <a class="volume" href="javascript:;"></a>
        </div>
        <audio v-el:audio
               @timeupdate="onTimeUpdateListener"
               @play="isPlayListener"
               @pause="isPlayListener"
               @ended="isPlayListener"
               id="jp_audio_0"
               preload="metadata"
               src="./static/1.mp3"></audio>
    </div>
</template>
<script>
    export default {
        data () {
            return {
                activePosition: 0,
                bufferedPosition: 0,
                currentTime: '00:00', // The initial current time
                duration: '00:00',
                isPause: true,
                isPlay: false,
                isSingle: false,
                clientX: null,
                isDrag: false,
                downPosition: null
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
            singleLoop () {
                if (this.$els.audio.loop === false) {
                    this.$els.audio.loop = true
                    this.isSingle = true
                } else {
                    this.$els.audio.loop = false
                    this.isSingle = false
                }
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
            handleMouseMove (e) {
                var _offset = (((e.clientX - this.clientX) / 315))
                var _activePosition = this.downPosition + _offset * 100
                if (_activePosition <= 0) {
                    this.activePosition = 0
                    this.$els.audio.currentTime = 0
                } else if (_activePosition >= 100) {
                    this.activePosition = 100
                    this.$els.audio.currentTime = this.$els.audio.duration * 1
                } else {
                    this.activePosition = _activePosition
                    this.$els.audio.currentTime = this.$els.audio.duration * (_activePosition / 100)
                }
            },
            setMouseDrag (e) {
                if (this.$els.audio.readyState === 4) {
                    var $this = this
                    this.isDrag = true
                    this.downPosition = this.activePosition
                    this.clientX = e.clientX
                    document.addEventListener('mousemove', $this.handleMouseMove, false)
                    document.addEventListener('mouseup', function (e) {
                        document.removeEventListener('mousemove', $this.handleMouseMove, false)
                    }, false)
                }
            },
            setPointPosition (e) {
                if (e.target.className !== 'point') {
                    var _proportion = (e.layerX / 315)
                    this.activePosition = _proportion * 100
                    this.$els.audio.currentTime = this.$els.audio.duration * _proportion
                }
            },
            onTimeUpdateListener () {
                var _audio = this.$els.audio
                var _currentTime = new Date(_audio.currentTime * 1000)
                this.currentTime = this.formatTime(_currentTime)
                this.bufferedPosition = (_audio.buffered.end(_audio.buffered.length - 1) / _audio.duration) * 100
                this.activePosition = (_audio.currentTime / _audio.duration) * 100
                if (_audio.ended === true) {
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
        cursor: pointer
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
    .active-progress{
        height: 6px;
        position: absolute;
        background: #3EDCFE;
        box-shadow: 0 1px 1px rgba(0, 0, 0, 0.3) inset;
        border-radius: 5px;
        z-index: 3;
    }
    .buffered-progress{
        height: 6px;
        position: absolute;
        box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1) inset;
        background: #e4e4e4;
        border-radius: 5px;
        z-index: 2;
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
        border-radius: 47px;
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
        border-radius:23px;
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
    .left-side{
        display: flex;
        justify-content: flex-start;
        align-items: center;
        width: 70px;
        height: 33px;
    }
    .right-side{
        display: flex;
        justify-content: flex-end;
        align-items: center;
        width: 70px;
        height: 33px;
    }
    .loop{
        width: 24px;
        height: 24px;
    }
    .single-loop{
        background: url(./img/single_loop.svg);
    }
    .all-loop{
        background: url(./img/all_loop.svg);
    }
    .volume{
        width: 24px;
        height: 24px;
        background: url(./img/volume-normal.svg);
    }
    .volume:hover{
        background: url(./img/volume-active.svg);
    }
</style>