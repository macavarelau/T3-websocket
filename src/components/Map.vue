<template>
  <div class="container">
    <div class="title"><b>Mapa "En Vivo"</b></div>
    <div class="map-container">
      <l-map :minZoom="1.9" :maxZoom="20" ref="myMap" @ready="mapReady()">
        <l-tile-layer
          :noWrap="true"
          :url="url"
          :attribution="attribution"
        ></l-tile-layer>

        <l-marker
          v-for="avion in positions"
          :key="avion[2]"
          :latLng="avion[0]"
          :icon="icon"
        >
          <l-tooltip> {{ avion[3] }} </l-tooltip>
        </l-marker>
      </l-map>
    </div>
  </div>
</template>

<script>
import L from "leaflet";
import { LMap, LTileLayer, LMarker, LTooltip } from "vue2-leaflet";

export default {
  name: "Map",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LTooltip,
  },
  data() {
    return {
      positions: [],
      center: L.latLng(0, 0),
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      icon: L.icon({
        iconUrl:
          "https://gogeticon.net/files/666745/f6ecb8344350e16e63ba6c78703df5a4.png",
        iconSize: [32, 37],
        iconAnchor: [16, 37],
      }),
    };
  },
  props: {
    flightPosition: Object,
    flightsList: Array,
    cantidadActualizaciones: Number,
  },
  methods: {
    mapReady() {
      var bounds = L.latLngBounds([
        [82.801708, -179.957095],
        [-70.355751, 179.556615],
      ]);
      this.map = this.$refs.myMap.mapObject;
      this.map.setMaxBounds(bounds);
      this.paintPath();
    },
    paintPath() {
      function sleep(time) {
        return new Promise((resolve) => setTimeout(resolve, time));
      }
      sleep(1000).then(() => {
        this.pLineGroup = L.layerGroup();
        for (let i = 0; i < this.flightsList.length; i++) {
          this.pLineGroup.addLayer(
            L.polyline(
              [this.flightsList[i].origin, this.flightsList[i].destination],
              { color: this.getRandomColor() }
            )
          );
        }
        this.pLineGroup.addTo(this.map);
      });
    },
    getRandomColor() {
      var letters = "0123456789ABCDEF";
      var color = "#";
      for (var i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      return color;
    },
  },
  mounted() {
    this.$nextTick(() => {
      this.$refs.myMap.mapObject;
    });
  },
  watch: {
    cantidadActualizaciones: function () {
      let list = [];
      Object.keys(this.flightPosition).forEach((llave) => {
        this.flightPosition[llave].push(llave);
        list.push(this.flightPosition[llave]);
      });
      this.positions = list;
      this.$forceUpdate();
    },
  },
};
</script>

<style scoped>
.container {
  padding: 10px;
  margin-right: 20px;
  width: 70%;
  height: 100%;
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
.map-container {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
}
l-icon {
  width: 100%;
}
</style>
