<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" @click="showTime">
    <img alt="Vue logo" src="./assets/logo.png" @click="showTime2">
    <DateTimePicker ref="picker1" ref-name="pickera" v-if="modelVisble" :callBack="confirm" @cancel="cancel"></DateTimePicker>
    <DateTimePicker ref="picker2" ref-name="pickerb" v-if="modelVisble2" :callBack="confirm" @cancel="cancel" :hourArea="['9:00', '20:00']"></DateTimePicker>
  </div>
</template>

<script>
import DateTimePicker from './dateTime/index.vue'

export default {
  name: 'app',
  data(){
    return{
      modelVisble: false,
      modelVisble2: false
    }
  },
  components: {
    DateTimePicker
  },
  methods:{
    showTime(){
      this.modelVisble = true
      this.$nextTick(()=>{
        this.$refs.picker1.init()
      })
    },
    confirm(){
      this.modelVisble = false
      this.modelVisble2 = false
    },
    cancel() {
      this.modelVisble = false
      this.modelVisble2 = false
    },
    showTime2() {
      this.modelVisble2 = true
      this.$nextTick(()=>{
        this.$refs.picker2.init()
      })
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
