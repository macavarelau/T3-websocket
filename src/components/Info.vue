<template>
  <div class="container">
    <div class="title"><b>Información de vuelos</b></div>
    <div v-for="info in infoList" :key="info.code" class="info-container">
      <div id="flight">
        <b>VUELO: {{ info.code }}</b>
      </div>
      <div id="details">
        Aerolínea: {{ info.airline }}<br />
        Avión: {{ info.plane }}<br />
        N° de asientos: {{ info.seats }}<br />
        Pasajeros:
        <div class="passengers">
          <li
            v-for="passenger in info.passengers"
            :key="passenger.name + passenger.age"
          >
            {{ passenger.name }} ({{ passenger.age + " años" }})
          </li>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Info",
  data() {
    return {};
  },
  props: {
    infoList: Array,
  },
  methods: {},
};
</script>

<style scoped>
.container {
  margin-left: 30px;
  margin-right: 20px;
  padding: 10px;
  width: 30%;
  height: 100%;
  display: flex;
  flex-direction: column;
  background-color: rgb(245, 245, 245);
  box-shadow: 2px 2px 10px #999;
  border-radius: 5px;
  overflow-y: scroll;
  align-items: center;
}
.title {
  padding: 12px;
  padding-top: 10px;
}
.info-container {
  margin: 10px;
  padding: 10px;
  width: 70%;
  height: 50%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  background-color: rgb(255, 255, 255);
  box-shadow: 2px 2px 10px #999;
  border-radius: 5px;
}
#flight {
  padding: 5px;
  background-color: rgb(166, 211, 216);
  border-radius: 5px;
}
#details {
  text-align: left;
  padding: 15px 7px 15px 15px;
  margin-top: 10px;
  background-color: rgb(231, 231, 223);
  border-radius: 5px;
}
.passengers {
  margin-left: 10px;
  margin-top: 10px;
  font-size: 12px;
}
</style>
