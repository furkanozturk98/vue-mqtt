<template>
  <div class="card">
     <div class="card-body">
       <div class="row">
      <div class="col-4">
        <div class="mb-2">
          Humidity: {{ data.humidity || 0 }} <br>
        </div>
        <div class="mb-2">
          Temperature: {{ data.temperature || 0 }} <br>
        </div>
        <div class="mb-2">
          Brightness: {{ data.brightness || 0 }} <br>
        </div>
      </div>
       <div class="col-8">
         <img v-if="lightStatus" src="../assets/on.png" height="250" class="ml-5" alt="">

         <img v-else src="../assets/off.png" height="250" class="ml-5" alt="">
       </div>
     </div>

     </div>
  </div>
</template>

<script>

export default {
  name: 'App',

  data() {
    return {
      data: {
        humidity: 0,
        temperature: 0,
        brightness: 0
      },

      lightStatus: false,

      client:false
    }
  },

  created() {
    const host = 'localhost'
    const port = '9001'
    const clientId = `mqtt_${Math.random().toString(16).slice(3)}`

    const connectUrl = `ws://${host}:${port}`

     this.client = Window.mqtt.connect(connectUrl, {
      clientId,
      clean: true,
      connectTimeout: 4000,
      reconnectPeriod: 1000,
    })

    this.subscribe('sensor/telemetry');

    this.subscribe('lamb/telemetry');
  },

  methods: {
    subscribe(topic) {
      this.client.on('connect', () => {
        console.log('Subscriber connected')

        this.client.subscribe(topic, () => {
          console.log(`Subscribe to topic ` + topic)
        })

        let self = this;
        this.client.on('message', (topic, payload) => {
          let data = JSON.parse(payload.toString());

          if(data.status === true || data.status === false){
            self.lightStatus = data.status
          }
          else{
            self.data = data;
          }

          console.log('Received Message:', topic, payload.toString())

        })
      })
    },
  }
}
</script>