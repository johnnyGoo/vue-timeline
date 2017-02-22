<template>
    <div>
    </div>
</template>


/*!
* vue-bar v0.0.1 (https://github.com/johnnyGoo/vue-timeline)
* Author: Johnny chen
*
* Copyright 2013-2016 Johnny chen
*/
<script>


    var count = 0;

    if (!window.Smart) {
        throw 'VueTimeline required smart.js'
    }
    var Smart = window.Smart;
    var Css = Smart.Css;
    var _ = Smart._;
    var Utils = Smart.Utils;
    var tween;

    // 注册
    export default {
        // 声明 props
        props: {
            playing: {
                type: Boolean,
                default: false
            },
            currentTime: {
                type: Number,
                default: 0
            },
            totalTime: {
                type: Number,
                default: 0
            }, loop: {
                type: Boolean,
                default: false
            },
            frameTime: {
                type: Number,
                default: 0.05
            }
        },
        data: function () {
            return {
                iv: 0
            }

        },
        watch: {
            playing: function (nv, ov) {
                if (nv) {
                    this.play()
                }else{
                    this.stop()
                }
            }
        }
        ,
        methods: {
            nextFrame: function () {
                if (this.currentTime === this.totalTime && this.loop) {
                    this.currentTime = 0;
                } else {
                    this.currentTime += this.frameTime;
                }

                if (this.currentTime >= this.totalTime) {
                    this.currentTime = this.totalTime;
                    if (!this.loop) {
                        this.stop();
                    } else {
                        this.autoNext()
                    }
                } else {
                    this.autoNext()
                }
                //  this.currentTime=(this.currentTime).toFixed(3);

                //   console.log(this.currentTime)


            },
            autoNext: function () {
                clearTimeout(this.iv);
                this.iv = setTimeout(this.nextFrame, this.frameTime * 1000);
            },
            play: function () {
                this.playing = true;
                this.autoNext();
                this.$emit('play', this);
            },
            stop: function () {
                clearTimeout(this.iv);
                this.playing = false;
                this.$emit('stop', this);
            }
        },
        computed: {},
        ready: function () {
            var me = this;
            if (me.playing) {
                me.play();
            }


        }


    }


</script>