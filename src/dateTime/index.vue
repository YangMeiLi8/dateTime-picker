<template>
  <div class="mf-picker">
    <div class="mf-picker-panel">
      <div class="mf-picker-choose border-bottom-1px">
        <span class="cancel" @click="cancel($event)">取消</span>
        <span class="confirm" ref="confirm">确定</span>
        <h1 class="mf-picker-title">选择时间</h1>
      </div>
      <div class="mf-picker-content">
        <div class="mask-top border-bottom-1px"></div>
        <div class="mask-bottom border-top-1px"></div>
        <div class="wheel-wrapper" :ref="refName">
          <div class="wheel">
            <ul class="wheel-scroll diff">
            </ul>
          </div>
          <div class="wheel">
            <ul class="wheel-scroll">
            </ul>
          </div>
          <div class="wheel">
            <ul class="wheel-scroll">
            </ul>
          </div>
        </div>
      </div>
      <div class="mf-picker-footer"></div>
    </div>
  </div>
</template>
<script>
  import {datePicker} from './index'
  export default{
    props:{
      refName: {
        type: String,
        default: 'wheel'
      },
      afterDay:{
        type: Number,
        default: 0
      },
      appointDays:{
        type: Number,
        default: 60
      },
      minuteLater:{
        type: Number,
        default: 120
      },
      hourArea:{
        type: Array,
        default: () => ['8:00', '19:30']
      },
      interval:{
        type: Number,
        default: 1
      },
      callBack:{
        type: Function,
        default: (timeStr, timeStamp) => {}
      }
    },
    mounted() {
    },
    methods: {
      cancel(e){
        e.preventDefault()
        e.stopPropagation()
        this.$emit('cancel')
      },
      init() {
        console.log(11)
        datePicker({
          wheel: this.$refs[this.refName],
          afterDay: this.afterDay, // 第几天后开始可以预约，默认是今天
          appointDays: this.appointDays, // 默认可以预约未来7天
          minuteLater: this.minuteLater, // 当天的话，默认只能预约20分钟之后,如果两个小时就填120
          hourArea: this.hourArea, // 预约小时可选的范围，默认是9：00-22：00
          interval: this.interval, // 分钟的间隔，默认一分钟
          // 点击确认获取到的时间戳和时间字符串
          callBack: this.callBack,
          confirmDom : this.$refs.confirm
        })
      }
    }
  }
</script>
<style scoped>
* {
  padding: 0;
  margin: 0;
}
.mf-picker {
  position: fixed;
  left: 0;
  top: 0;
  z-index: 100;
  width: 100%;
  height: 100%;
  overflow: hidden;
  text-align: center;
  font-size: 14px;
  background-color: rgba(37, 38, 45, .4);
}

.mf-picker .mf-picker-panel {
  position: absolute;
  z-index: 600;
  bottom: 0;
  width: 100%;
  height: 273px;
  background: #fff;
}

.mf-picker .mf-picker-panel .mf-picker-choose {
  position: relative;
  height: 60px;
  color: #999;
}

.mf-picker .mf-picker-panel .mf-picker-content {
  position: relative;
  top: 20px;
  height: 173px;
}

.border-bottom-1px,
.border-top-1px {
  position: relative;
}

.border-bottom-1px:after,
.border-top-1px:before {
  content: "";
  display: block;
  position: absolute;
  -webkit-transform-origin: 0 0;
  transform-origin: 0 0;
}

.border-bottom-1px:after {
  border-bottom: 1px solid #ebebeb;
  left: 0;
  bottom: 0;
  width: 100%;
  -webkit-transform-origin: 0 bottom;
  transform-origin: 0 bottom;
}

.mf-picker .mf-picker-panel .mf-picker-choose .cancel {
  left: 0;
}

.mf-picker .mf-picker-panel .mf-picker-choose .confirm {
  right: 0;
  color: #fc9153;
}

.mf-picker .mf-picker-panel .mf-picker-choose .mf-picker-title {
  margin: 0;
  line-height: 60px;
  font-weight: 400;
  text-align: center;
  font-size: 18px;
  color: #333;
}

.mf-picker .mf-picker-panel .mf-picker-choose .cancel,
.mf-picker .mf-picker-panel .mf-picker-choose .confirm {
  position: absolute;
  top: 6px;
  padding: 16px;
  font-size: 14px;
}

.mf-picker .mf-picker-panel .mf-picker-content .mask-top {
  position: absolute;
  top: 0;
  background: -webkit-linear-gradient(bottom, hsla(0, 0%, 100%, .4), hsla(0, 0%, 100%, .8));
  background: linear-gradient(to bottom, hsla(0, 0%, 100%, .4), hsla(0, 0%, 100%, .8));
}

.mf-picker .mf-picker-panel .mf-picker-content .mask-bottom {
  position: absolute;
  bottom: 1px;
  background: -webkit-linear-gradient(top, hsla(0, 0%, 100%, .4), hsla(0, 0%, 100%, .8));
  background: linear-gradient(to top, hsla(0, 0%, 100%, .4), hsla(0, 0%, 100%, .8));
}

.mf-picker .mf-picker-panel .mf-picker-content .mask-bottom,
.mf-picker .mf-picker-panel .mf-picker-content .mask-top {
  z-index: 10;
  width: 100%;
  height: 68px;
  pointer-events: none;
  -webkit-transform: translateZ(0);
  transform: translateZ(0);
}

.border-top-1px:before {
  border-top: 1px solid #ebebeb;
  left: 0;
  top: 0;
  width: 100%;
}

.mf-picker .mf-picker-panel .wheel-wrapper {
  display: -webkit-box;
  display: flex;
  display: -webkit-flex;
  padding: 0 16px;
}

.mf-picker .mf-picker-panel .wheel-wrapper .wheel {
  -webkit-box-flex: 1;
  flex: 1;
  -webkit-flex: 1;
  flex-basis: auto;
  width: 1%;
  height: 173px;
  overflow: hidden;
  font-size: 20px;
}


.mf-picker .mf-picker-panel .wheel-wrapper .wheel .wheel-scroll {
  padding: 0;
  margin-top: 68px;
  line-height: 36px;
  list-style: none;
  -webkit-transition-timing-function: cubic-bezier(0.165, 0.84, 0.44, 1);
  transition-timing-function: cubic-bezier(0.165, 0.84, 0.44, 1);
}

.mf-picker .mf-picker-panel .wheel-wrapper .wheel .diff {
  -webkit-transition-duration: 150ms;
  transition-duration: 150ms;
}

.mf-picker .mf-picker-panel .wheel-wrapper .wheel .wheel-scroll .wheel-item {
  -webkit-transition-timing-function: cubic-bezier(0.165, 0.84, 0.44, 1);
  transition-timing-function: cubic-bezier(0.165, 0.84, 0.44, 1);
}
</style>