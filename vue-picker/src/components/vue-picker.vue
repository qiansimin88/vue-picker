<template>
    <div class="picker">
        <!--遮罩-->
        <div class="mask" v-show = "show">
        </div>
        <!--弹框部分-->
        <div class="pick-wrap" v-show = "show">
            <div class="pick-btn">
                <button @click="cancle">
                     取消
                </button>
                <button @click="confirm">
                     确定
                </button>
            </div>
            <div class="pick-select"  @touchstart.stop= 'touchstartHandle' @touchmove.prevent = "touchmoveHandle" @touchend = 'touchendHandle'> 
                <div class='middle-line'>
                    <div class="slot-wrap" v-el:slot :style = "{transform: `translate3D(0, ${top}px, 0)`}">
                         <div class="pick-item" v-for="(i, o) in value" v-text="o" :class="{selected: -changeSelect == i}">
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<style lang='less' scoped>
    .mask {
        height: 100vh; width: 100%; background: rgba(0, 0, 0, 0.6); position: fixed; top:0; left: 0; z-index: 444;
    }
    .pick-wrap {
        position: fixed; z-index: 445; bottom: 0; left: 0; background: #fff; width: 100%; 
        .pick-btn {
            border-bottom: 1px solid #eaeaea; font-size: .28rem; height: .8rem; display: flex; justify-content: space-between;
            button {
                font-size: .28rem; width: 50%; display: block; text-align: center; color: #4ab7ed;
            }
        }
        .pick-select {
            position: relative; overflow: hidden; height: 500px;
            .middle-line {
                position: absolute; top: 50%; transform: translate3d(0, -50%, 0); height: 100px;  left: 0; width: 100%; background: #f0f0f0; pointer-events: none; z-index: -1;
            }
            .pick-item {
                height: 100px; font-size: .28rem; text-align: center;line-height: 100px;
                &.selected {
                    color:#4ab7ed;
                }
            }
            .slot-wrap {
                transition: transform .2s ease-out; position: absolute; left: 0; width: 100%; top: 0;
            }
        }
    }
</style>
<script>
    export default {
        name:'vue-picker',
        data () {
            return {
                startPageY: 0,   //触摸开始的 Y 轴坐标
                top: 0,
                singerHeight: 100, //单个高度
                initTop: 0,  //滑动之前的高度
                changeSelect: 0  //滑动的距离dan'wei
            }
        },
        props: {
            show: {
                type: Boolean,
                twoway:true,
                default: false,
                required: true
            },
            value: {
                type: Array,
                required: true
            },
            selectIndex: {
                twoway: true,
                type: Number,
                default: 0
            }
        },
        methods: {
            touchstartHandle (e) {
                this.initTop  = this.top;
                this.startPageY = e.touches[0].pageY;
            },
            touchmoveHandle (e) {
                let nowPageY = e.targetTouches[0].pageY;
                let dis = nowPageY - this.startPageY;
                this.top = this.initTop + dis;
            },
            touchendHandle (e) {
                this.handlePosition();
            },
            init () {
                this.top = -this.selectIndex * this.singerHeight;
                this.initTop = -this.selectIndex * this.singerHeight;
                this.changeSelect = -this.selectIndex;
            },
            handlePosition() {
                let changeSelect = this.changeSelect = Math.round(this.top / this.singerHeight);
                if(changeSelect > 0) this.changeSelect = 0;
                if(changeSelect < (-this.value.length + 1)) this.changeSelect = -this.value.length + 1
                this.top = this.changeSelect * this.singerHeight;
            },
            cancle () {
                this.show = false;
                this.top = -this.selectIndex * this.singerHeight;
                this.changeSelect = -this.selectIndex;
            },
            confirm () {
                this.show = false;
                this.selectIndex = -this.changeSelect;
            }
        },
        ready () {
            this.init();
        }
    }
</script>