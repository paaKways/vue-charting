<template>
  <q-page class="flex flex-center">
    Chart here

    <button onclick=''>Click me</button>
  </q-page>
</template>

<style>
</style>

<script>
import mqtt from 'mqtt'

export default {
  name: 'PageIndex',
  data: function () {
    return { level: 0, client: null }
  },

  created: function () {
    options = {
      port: 35587,
      username: 'uzyenhei',
      password: 'otG6lNz-TTnx'
    }
    const client = mqtt.connect('wss://m10.cloudmqtt.com',options )

    this.client = client

    client.on('connect', function () {
      client.subscribe('value', function (err) {
        if (!err) {
          client.publish('value', '200')
          console.log('test /value 200')
        }
      })
    })
    client.on('value', function (topic, message) {
      // message is Buffer
      console.log(message.toString())
      client.end()
    })
  },
  methods: {
  }
}
</script>
