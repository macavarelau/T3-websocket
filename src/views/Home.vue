<template>
  <div v-if="conectado" class="home">
    <div class="flights-container">
      <Info :infoList="infoList"></Info>
      <Map
        :flightsList="flightsList"
        v-if="hasKey"
        :flightPosition="position"
        :cantidadActualizaciones="updates"
      ></Map>
    </div>
    <Chat :messageList="chatList" @mensaje="sendMessage"></Chat>
  </div>
</template>

<script>
// @ is an alias to /src
import Chat from "../components/Chat.vue";
import Map from "../components/Map.vue";
import Info from "../components/Info.vue";
import io from "socket.io-client";

var socket = io("wss://tarea-3-websocket.2021-1.tallerdeintegracion.cl", {
  path: "/flights",
});

export default {
  name: "Home",
  data() {
    return {
      hasKey: false,
      position: {},
      flightsList: [],
      infoList: [],
      chatList: [],
      conectado: false,
      updates: 0,
    };
  },
  components: {
    Chat,
    Map,
    Info,
  },
  methods: {
    sendMessage(msj) {
      socket.emit("CHAT", msj);
    },
  },
  created() {
    socket.connect();
    socket.on("CHAT", (data) => {
      let chatData = data;
      let fecha = new Date(chatData.date).toLocaleString();
      chatData.fecha = fecha;
      this.chatList.push(chatData);
      console.log(chatData);
    });
    socket.emit("FLIGHTS");
    socket.once("FLIGHTS", (data) => {
      this.flightsList = data;
      this.infoList = data;
    });
    socket.on("POSITION", (data) => {
      let flightPosition = data;
      if (Object.keys(this.position).includes(flightPosition.code)) {
        this.position[flightPosition.code][0] = flightPosition.position;
        this.position[flightPosition.code][1] =
          this.position[flightPosition.code][1] + 1;
        this.position[flightPosition.code][2] =
          flightPosition.code + this.position[flightPosition.code][1];
      } else {
        this.position[flightPosition.code] = [flightPosition.position, 0];
        this.position[flightPosition.code].push(flightPosition.code + 0);
      }
      this.hasKey = true;
      this.updates += 1;
    });
    this.conectado = true;
  },
};
</script>
<style scoped>
.home {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  height: 100%;
  width: 100%;
}
.flights-container {
  display: flex;
  flex-direction: row;
  width: 100%;
  height: 85%;
}
</style>
