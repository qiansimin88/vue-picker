<template>
  <div id="app">
  <div @click="show = true">点击</div>
  {{slots[select]}}
  <picker :show.sync="show" :slots="slots"></picker>
  </div>
</template>

<script>
import picker from './components/vue-picker.vue'

export default {
  data () {
    return {
      show: true,
      select:2,
      slots : [
        {
          value: ['a,b','c','d','e'],
          selectIndex: 0
        },
        {
          value: ['f','g','h','e','i','q','l'],
          selectIndex: 1
        }
      ]
    }
  },
  components: {
    picker
  }
}
</script>

<style>
html {
  height: 100%;
}

body {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}

#app {
  color: #2c3e50;
  margin-top: -100px;
  max-width: 600px;
  font-family: Source Sans Pro, Helvetica, sans-serif;
  text-align: center;
}

#app a {
  color: #42b983;
  text-decoration: none;
}

.logo {
  width: 100px;
  height: 100px
}
</style>
