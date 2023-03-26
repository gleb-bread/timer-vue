<template>
    <main>
        <div class="wrapper">
            <div
                class="container"
                v-for="t in timers"
                v-bind:key="t.id"
                v-bind:class="{
                    stop: t.sel == 0,
                }"
            >
                <div class="container__timer">
                    <span class="time"
                        >{{
                            t.time.hrs > 0
                                ? t.time.hrs + ':' + t.time.min
                                : t.time.min > 0
                                ? t.time.min + ':' + t.time.sec
                                : t.time.sec
                        }}
                    </span>
                </div>
                <hr class="borderline" />
                <div class="container__buttons">
                    <button
                        class="button__play"
                        v-bind:class="{
                            none: t.sel == 1,
                        }"
                        v-on:click="play(t)"
                    >
                        <img src="./img/play.svg" alt="Button Play" srcset="" />
                    </button>
                    <button
                        class="button__pause"
                        v-bind:class="{
                            none: t.sel == 0,
                        }"
                        v-on:click="stop(t)"
                    >
                        <img
                            src="./img/pause.svg"
                            alt="Button Pause"
                            srcset=""
                        />
                    </button>
                    <button class="button__restore" v-on:click="restore(t)">
                        <img
                            src="./img/restore.svg"
                            alt="Button Restore"
                            srcset=""
                        />
                    </button>
                </div>
            </div>
            <div class="container" v-on:click="add()">
                <button class="button__restore">
                    <img src="./img/add.svg" alt="Button Restore" srcset="" />
                </button>
            </div>
        </div>
    </main>
</template>

<script>
export default {
    name: 'App',

    data() {
        return {
            timers: [{ time: { sec: 0, min: 59, hrs: 0 }, id: 0, sel: 0 }],
        }
    },

    methods: {
        add() {
            const newTimer = {
                time: { sec: 0, min: 0, hrs: 0 },
                id: this.timers[this.timers.length - 1].id + 1,
                sel: 0,
            }

            this.timers.push(newTimer)
        },

        play(timer) {
            timer.sel = 1
            timer.counter = setInterval(() => {
                timer.time.sec++
                if (timer.time.sec == 60) {
                    timer.time.min++
                    timer.time.sec = 0
                }
                if (timer.time.min == 60) {
                    timer.time.hrs++
                    timer.time.min = 0
                }
            }, 1000)
        },
        stop(timer) {
            clearInterval(timer.counter)
            timer.sel = 0
        },
        restore(timer) {
            if (timer.sel == 1) {
                this.stop(timer)
                timer.time = { sec: 0, min: 0, hrs: 0 }
            } else {
                timer.time = { sec: 0, min: 0, hrs: 0 }
            }
        },
    },
}
</script>

<style src="./style.css"></style>
