<template>
  <div id="app">
  <div @click="show = true">点击</div>
  {{value[select]}}
  <picker :select-index.sync ='select' :show.sync="show" :value="value"></picker>
  </div>
</template>

<script>
import picker from './components/vue-picker.vue'

export default {
  data () {
    return {
      show: true,
      select:2,
      value : ['asdasd','呵呵呵',3,4,5,6,7.9,10,11,12]
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
