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
          value: [
            {
              id:1,
              hehe: 'hehe1'
            },
            {
              id:2,
              hehe: 'hehe2'
            },
            {
              id:3,
              hehe: 'hehe3'
            },
            {
              id:4,
              hehe: 'hehe4'
            },
            {
              id:5,
              hehe: 'hehe5'
            },
          ],
          selectIndex: 3,
          showfield: 'hehe'
        },
        {
          value: [
            {
              id:1,
              xixi: 'xixi1'
            },
            {
              id:2,
              xixi: 'xixi2'
            },
            {
              id:3,
              xixi: 'xixi3'
            },
            {
              id:4,
              xixi: 'xixi4'
            },
            {
              id:5,
              xixi: 'xixi5'
            },
          ],
          selectIndex: 1,
          showfield: 'xixi'
        }
      ]
    }
  },
  events: {
    confirm (a) {
      console.log(a)
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
