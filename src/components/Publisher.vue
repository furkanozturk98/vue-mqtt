<template>
  <div class="card">
      <div class="card-body">

        <div class="form-group">
          <label>Topic</label>
            <input type="text" class="form-control" v-model="topic">
        </div>

          <div class="form-group">
            <label>Humidity</label>
            <div class="form-inline">
              <input type="range" class="form-control" min="0" max="100" v-model="humidity"> <span class="ml-1"> {{ humidity }} % </span>
            </div>
          </div>

        <div class="form-group">
          <label>temperature</label>
          <div class="form-inline">
            <input type="range" class="form-control" min="0" max="100" v-model="temperature"> <span class="ml-1"> {{ temperature }} % </span>
          </div>
        </div>

        <div class="form-group">
          <label>Brightness</label>
          <div class="form-inline">
            <input type="range" class="form-control" min="0" max="100" v-model="brightness"> <span class="ml-1"> {{ brightness }} % </span>
          </div>
        </div>

        <button class="btn btn-secondary mb-2" @click="simulate">Simulate</button>

        <hr>

        <div class="form-group">
          <button class="btn btn-primary mr-2" @click="on">On</button>
          <button class="btn btn-danger" @click="off">Off</button>
        </div>
      </div>
  </div>
</template>

<script>

import * as client from "process";

export default {
  name: 'App',

  created(){
    const host = 'localhost'
    const port = '9001'
    //const clientId = `mqtt_${Math.random().toString(16).slice(3)}`

    const connectUrl = `ws://${host}:${port}`

     this.client = Window.mqtt.connect(connectUrl, {
      //clientId,
      clean: true,
      connectTimeout: 4000,
      reconnectPeriod: 1000,
    })

    client.on('connect', () => {
      console.log('Publisher connected')
    })
  },

  data(){
      return {
        topic: 'sensor/telemetry',
          humidity: 0,
          temperature: 0,
          brightness: 0
      }
  },

  methods: {
    simulate(){

      let message = {
        humidity: this.humidity,
        temperature: this.temperature,
        brightness: this.brightness
      }

      this.client.publish(this.topic, JSON.stringify(message), { qos: 0, retain: false }, (error) => {

      })
    },

    on(){
      let message = {
        status: true
      }

      this.client.publish('lamb/telemetry', JSON.stringify(message), { qos: 0, retain: false })
    },

    off(){
      let message = {
        status: false
      }

      this.client.publish('lamb/telemetry',  JSON.stringify(message), { qos: 0, retain: false })
    }
  }

}
</script>