<template>
  <div class="home">
    <v-flex pl-3 pb-4>
    <h1>Home page</h1>
    </v-flex>
    <v-layout row wrap pl-3>
      <v-flex xs5 md5>
        <iframe height="500px" width="670px" src="http://181.73.132.123:8080/video"></iframe>
      </v-flex>
      <v-flex xs7 md7 px-3>
        <v-data-table
        :headers="headers"
        :items="items"
        class="elevation-1"
        >
          <template v-slot:items="props">
            <td>{{ props.item.temperatura}}</td>
            <td>{{ props.item.humedad}}</td>
          </template>
        </v-data-table>
      </v-flex>
    </v-layout>
    <v-flex pl-3 pt-5>
      <span style="color:black">Ventilador</span>
      <v-btn :disabled = this.availability @click="sendMessage" color="info">Enviar</v-btn>
    </v-flex>
  </div>
</template>

<script>

// import mqtt from 'mqtt'
import Paho from 'paho-mqtt'
var client
export default {
  components: {
  },
  data() {
    return {
      headers:[
        {text:'Temperatura', value:'Temp',sortable:false,align: 'left'},
        {text:'Humedad', value:'Hum',sortable:false,align: 'left'}
      ],
      items:[],
      availability: false
    }
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
      console.log("Loggggggg",message.payloadString)
      /* eslint-disable no-console */
      if(message.payloadString != "Encender" && message.payloadString != "Ready") {
      var splited = message.payloadString.split(" ")
      /* eslint-disable no-console */
      console.log("temperatura",splited[1])
      /* eslint-disable no-console */
      /* eslint-disable no-console */
      console.log("humedad",splited[2])
      /* eslint-disable no-console */
      this.items.unshift({'temperatura':splited[1],'humedad':splited[2]})
      /* eslint-disable no-console */
      console.log(this.items)
      /* eslint-disable no-console */
      }
      if(message.payloadString == "Ready"){
        this.availability = false
      } 
    },
    onConnect() {
      /* eslint-disable no-console */
      console.log("conectado")
      /* eslint-disable no-console */
      client.subscribe("esp/test")
      // var message = new Paho.Message("Hello")
      //message.destinationName = "esp/test"
      //client.send(message)
    },
    sendMessage() {
      client.subscribe("esp/test")
      var message = new Paho.Message("Encender")
      message.destinationName = "esp/test"
      client.send(message)
      this.availability = true
    }
  },
    mounted() {
      this.connectMqtt()
  }
}
</script>
