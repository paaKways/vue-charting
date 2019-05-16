<template>
  <q-page class="flex flex-center">
        <div id="chart"></div>
       <vue-plotly id="graph" :data="data" :layout="layout" :options="options" />
  </q-page>
</template>

<style>
</style>

<script>
import Vue from 'vue'
import VueMqtt from 'vue-mqtt'
import VuePlotly from '@statnett/vue-plotly/dist/vue-plotly.js'

const options = {
  port: 35587,
  username: 'uzyenhei',
  password: 'otG6lNz-TTnx'
}

Vue.use(VueMqtt, 'wss://m10.cloudmqtt.com', options)


export default {
  name: 'PageIndex',
  components: {
      VuePlotly
  },
  data () {
      return {
          data: [{ x: [1,3], y: [2,4] }],
          layout: {},
          options: {}
      }
  },

  
  created: function () {
    this.$mqtt.subscribe('waterLevelValue', options)
    console.log('connected')
  },
  mqtt: {
    'waterLevelValue': function (val) {
        var n = val.toString()
        console.log('New value: ', n)
        var new_data = this.data[0]['y']
        new_data.push(parseInt(n))
        VuePlotly.update(new_data)
    }
  },

 
  methods: {
    getCurrentTime () {
        let today = new Date();
        return today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
    }
 
  }
}
</script>
