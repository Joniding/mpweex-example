<template>
    <div>
        <div class="group center">
            <div class="panel">
                <div style="flex-direction: row;">
                    <text class="text lab">次数：</text>
                    <text class="text">{{times}}</text>
                </div>
                <div style="flex-direction: row;">
                    <text class="text lab">x：</text>
                    <text class="text">{{x}}</text>
                </div>
                <div style="flex-direction: row;">
                    <text class="text lab">y：</text>
                    <text class="text">{{y}}</text>
                </div>
                <div style="flex-direction: row;">
                    <text class="text lab">z：</text>
                    <text class="text">{{z}}</text>
                </div>
            </div>
        </div>
        <div class="group">
            <mpwx-btn class="btn" label='获取' id="0" @onBtnClicked="get"></mpwx-btn>
            <mpwx-btn class="btn" label='监听' id="1" @onBtnClicked="watch"></mpwx-btn>
            <mpwx-btn class="btn" label='停止' id="2" @onBtnClicked="clearWatch"></mpwx-btn>
        </div>
    </div>
</template>

<script>
    const accelerometer = weex.requireModule('accelerometer');
    import MpwxBtn from "../mpwx-btn"
    export default {
        components : {MpwxBtn},
        data () {
            return {
                times: 0,
                x: '---',
                y: '---',
                z: '---'
            }
        },
        methods: {
            get () {
                accelerometer.get(event => {
                    if(event.result == 'ok') {
                        this.times++;
                        this.x = event.x;
                        this.y = event.y;
                        this.z = event.z;
                    }
                });
            },
            watch() {
                accelerometer.watch(event => {
                    if(event.result == 'ok') {
                        this.times++;
                        this.x = event.x;
                        this.y = event.y;
                        this.z = event.z;
                    }
                });
            },
            clearWatch() {
                accelerometer.clearWatch();
            }
        }
    }
</script>

<style scoped>
    .panel {
        flex-direction: column;
        justify-content: center;
        border-width: 2px;
        border-style: solid;
        border-color: rgb(162, 217, 192);
        background-color: rgba(162, 217, 192, 0.2);
        padding-top: 10px;
        padding-bottom: 10px;
    }
    .group {
        flex-direction: column;
        width: 650px;
        margin-left: 50px;
        margin-top: 50px;
    }
    .center {
        justify-content: center;
    }
    .text {
        font-size: 32px;
        text-align: center;
        padding-left: 25px;
        padding-right: 25px;
        color: #41B883;
    }
    .lab {
        width: 250px;
        text-align: right;
    }
    .btn {
        margin-bottom: 12px;
        margin-top: 12px;
    }
</style>