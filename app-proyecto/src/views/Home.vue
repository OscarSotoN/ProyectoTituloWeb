<template>
  <div class="home">
    <h1>Home page</h1>
    <v-text-field label="Mensaje"></v-text-field>
    <v-btn @click="sendMessage" color="info">Enviar</v-btn>
    <iframe src="http://181.73.132.123:8080/video"></iframe>
  </div>
</template>

<script>

// import mqtt from 'mqtt'
import Paho from 'paho-mqtt'
var client
export default {
  components: {
  },
  methods: {
    connectMqtt() {
      /* eslint-disable no-console */
      console.log("before connect")
      /* eslint-disable no-console */
      client = new Paho.Client("postman.cloudmqtt.com", 37679,"oscar");
      client.onConnectionLost = this.onConnectionLost
      client.onMessageArrived = this.onMessageArrived
      client.connect({onSuccess:this.onConnect, useSSL:true, userName: "elpixqob", password:"mGjfSKZV51cd"});
      /* eslint-disable no-console */
      console.log("after connect")
      /* eslint-disable no-console */
      // client = mqtt.connect('wss://postman.cloudmqtt.com:37679/mqtt')
      
    },
    onConnectionLost(responseObject) {
      /* eslint-disable no-console */
      console.log("Se perdio la connexion")
      /* eslint-disable no-console */
      if (responseObject.errorCode !== 0) {
        /* eslint-disable no-console */
        console.log("onConnectionLost:"+responseObject.errorMessage)
        /* eslint-disable no-console */
      }
    },
    onMessageArrived(message) {
      /* eslint-disable no-console */
      console.log(message.payloadString)
      /* eslint-disable no-console */
    },
    onConnect() {
      /* eslint-disable no-console */
      console.log("conectado")
      /* eslint-disable no-console */
      client.subscribe("esp/test")
      var message = new Paho.Message("Hello")
      message.destinationName = "esp/test"
      client.send(message)
    },
    sendMessage() {
      /* eslint-disable no-console */
      console.log("clic")
      /* eslint-disable no-console */
      client.subscribe("esp/test")
      var message = new Paho.Message("Encender")
      message.destinationName = "esp/test"
      client.send(message)
    }
  },
    mounted() {
      this.connectMqtt()
  }
}
</script>
