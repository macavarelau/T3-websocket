<template>
  <div class="chat-container">
    <div class="title"><b>Centro de Control</b></div>
    <div class="read-container">
      <div class="chat-box">
        <div
          class="chat-msg"
          :class="{ mine: isMine(message) }"
          v-for="message in messageList"
          :key="message.date"
        >
          <p class="username">{{ message.name + ":" }}</p>
          <p class="msj">{{ message.message }}</p>
          <p class="date">{{ message.fecha }}</p>
        </div>
      </div>
    </div>
    <div class="write-container">
      <div class="user">
        <label for="name">Nombre: </label>
        <input type="text" maxlength="20" name="name" v-model="name" />
      </div>
      <div class="message">
        <textarea
          name="message"
          placeholder="Escribe tu mensaje..."
          maxlength="1000"
          cols="30"
          rows="10"
          v-model="msj"
        ></textarea>
      </div>
      <button @click="sendMessage">Enviar</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Chat",
  data() {
    return {
      name: "",
      msj: "",
      mensajesEnviados: [],
    };
  },
  props: {
    messageList: Array,
  },
  methods: {
    sendMessage() {
      let obj = { name: this.name, message: this.msj };
      this.$emit("mensaje", obj);
      let msgCompilado = this.name + this.msj;
      this.mensajesEnviados.push(msgCompilado);
      this.msj = "";
    },
    isMine(message) {
      let compilado = message.name + message.message;
      if (this.mensajesEnviados.includes(compilado)) return true;
      else return false;
    },
  },
};
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
.chat-container {
  margin-right: 30px;
  padding: 10px;
  width: 20%;
  height: 85%;
  display: flex;
  flex-direction: column;
  background-color: rgb(245, 245, 245);
  box-shadow: 2px 2px 10px #999;
  border-radius: 5px;
}
.title {
  padding: 12px;
  padding-top: 10px;
}
.read-container {
  overflow-y: scroll;
  padding: 10px;
  height: 80%;
  border: 1px solid rgb(226, 226, 226);
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  background-color: white;
  border-radius: 5px;
  word-wrap: break-word;
}
.read-container .chat-msg {
  margin-top: 10px;
  width: 70%;
  text-align: left;
  padding: 7px;
  background-color: rgb(235, 235, 235);
  border-radius: 5px;
}
.read-container .mine {
  width: auto;
  padding: 7px;
  margin-left: 30%;
  background-color: rgb(224, 241, 212);
  border-radius: 5px;
}
.write-container {
  padding: 10px;
  height: 20%;
}
.user {
  padding-top: 5px;
  padding-bottom: 5px;
  display: flex;
  justify-content: flex-start;
}
.user label {
  padding-right: 10px;
  font-weight: bold;
}
.message {
  padding-top: 5px;
  padding-bottom: 5px;
  display: flex;
  width: 100%;
  height: 50%;
}
.message textarea {
  width: 100%;
  text-align: start;
}
.chat-msg .username {
  margin-top: 1px;
  margin-bottom: 0;
  font-weight: bold;
}
.chat-msg .msj {
  margin-top: 5px;
  margin-bottom: 5px;
}
.chat-msg .date {
  margin-top: 0;
  margin-bottom: 0;
  color: lightslategray;
  font-size: 12px;
  text-align: right;
}
button {
  padding: 10px;
  display: unset;
  flex-direction: column;
  justify-content: flex-end;
}
</style>
