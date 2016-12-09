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
            <div class="pick-select"> 
                <div class='middle-line'>
                    <div v-for = "(x, j) in slots" class="slot-wrap" v-el:slot :style = "{transform: 'translate3D(0, ' + allOptions[x].top + 'px, 0)'}">
                         <div class="pick-item" v-for="(i, o) in j.value" v-text="o[j.showfield]" :class="{selected: -allOptions[x].changeSelect == i}">
                        </div>
                    </div>
                </div>
                <div class="touch-area" v-for = "(z, y) in slots"  @touchstart.stop= 'touchstartHandle($event, z)' @touchmove.prevent = "touchmoveHandle($event, z)" @touchend = 'touchendHandle($event, z)'>
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
            position: relative; overflow: hidden; height: 500px; display: flex; 
            .touch-area {
                flex: 1; height: 100%;
            }
            .middle-line {
                position: absolute; top: 50%; transform: translate3d(0, -50%, 0); height: 100px;  left: 0; width: 100%; background: #f0f0f0; pointer-events: none; z-index: -1; display: flex; 
            }
            .pick-item {
                height: 100px; font-size: .28rem; text-align: center;line-height: 100px;
                &.selected {
                    color:#4ab7ed;
                }
            }
            .slot-wrap {
                transition: transform .2s ease-out;flex: 1;
            }
        }
    }
</style>
<script>
    export default {
        name:'vue-picker',
        data () {
            return {
                singerHeight: 100, //单个高度
                allOptions: []   //所有的属性和变化的容器
            }
        },
        props: {
            show: {
                type: Boolean,
                twoway:true,
                default: false,
                required: true
            },
            slots: {                    //slots 里面 value 负责放数据对象,数组对象形式    showfield负责要显示的字段
                type: Array,
                required: true
            }
        },
        methods: {
            touchstartHandle (e, i) {
                let allOptions = this.allOptions[i];
                allOptions.initTop = allOptions.top;
                allOptions.startPageY = e.touches[0].pageY;
            },
            touchmoveHandle (e, i) {
                let nowPageY = e.targetTouches[0].pageY;
                let dis = nowPageY - this.allOptions[i].startPageY;
                this.allOptions[i].top = this.allOptions[i].initTop + dis;
            },
            touchendHandle (e, i) {
                this.handlePosition(e, i);
            },
            init () {
                //配置
                this.slots.map((o, i) => {
                    //如果传过来的是字符串的话
                    let selectIndex = o.selectIndex;
                    this.allOptions.push({
                        selectIndex: selectIndex,
                        changeSelect: 0,  //滑动的距离单位
                        startPageY: 0,      
                        top: 0,         //当前的高度
                        initTop: 0      //滑动之前当前模块的top
                    });
                    let allOptions = this.allOptions[i];
                    allOptions.top = -allOptions.selectIndex * this.singerHeight;
                    allOptions.initTop = -allOptions.selectIndex * this.singerHeight;
                    allOptions.changeSelect = -allOptions.selectIndex;
                });
            },
            handlePosition(e, i) {
                let allOptions = this.allOptions[i];
                let changeSelect = allOptions.changeSelect = Math.round(allOptions.top / this.singerHeight);
                if(changeSelect > 0) allOptions.changeSelect = 0;
                if(changeSelect < (-this.slots[i].value.length + 1)) allOptions.changeSelect = -this.slots[i].value.length + 1
                allOptions.top = allOptions.changeSelect * this.singerHeight;    
            },
            cancle () {
                this.show = false;
                this.allOptions.map((o, i) => {
                    o.top = -o.selectIndex * this.singerHeight;
                    o.changeSelect = -o.selectIndex;
                });
            },
            confirm () {
                this.show = false;
                 this.allOptions.map((o, i) => {
                    o.selectIndex = -o.changeSelect;
                });

                let selectArray = _ => {
                    return this.slots.map((o, i) => {
                        return o.value[this.allOptions[i].selectIndex];
                    })
                }
                this.$dispatch('confirm', selectArray())
            }
        },
        created () {
            this.init();
        }
    }
</script>