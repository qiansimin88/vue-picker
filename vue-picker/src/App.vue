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
      show: false,
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
          selectIndex: 0,
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
          selectIndex: 0,
          showfield: 'xixi'
        },
        {
          value: [
            {
              id:1,
              dd: 'dd1'
            },
            {
              id:2,
              dd: 'dd2'
            },
            {
              id:3,
              dd: 'dd3'
            },
            {
              id:4,
              dd: 'dd4'
            },
            {
              id:5,
              dd: 'dd5'
            },
          ],
          selectIndex: 0,
          showfield: 'dd'
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
</style>
