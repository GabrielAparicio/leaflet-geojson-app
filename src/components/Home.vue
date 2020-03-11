<template>
  <l-map
    class="maps"
    :bounds="bounds"
    @click="addMarker"
    :options="{ zoomControl: false }"
  >
    <l-tile-layer :url="url"></l-tile-layer>
    <l-control-zoom position="bottomright"></l-control-zoom>
    <l-geo-json :geojson="geojson" @add="setGeojsonBounds"></l-geo-json>
  </l-map>
</template>

<script>
import { LMap, LTileLayer, LGeoJson, LControlZoom } from "vue2-leaflet";
import { geojsonData } from "../geojsonData";

export default {
  name: "Home",
  components: {
    LMap,
    LTileLayer,
    LGeoJson,
    LControlZoom
  },
  data() {
    return {
      loading: true,
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      geojson: null,
      polygon: null,
      bounds: null
    };
  },
  methods: {
    addMarker({ latlng }) {
      const { lat, lng } = latlng;
      console.log(lat, lng);
    },
    setGeojsonBounds(e) {
      this.bounds = e.target.getBounds();
    }
  },
  computed: {},
  created() {
    this.geojson = geojsonData;
    this.polygon = this.geojson.features[0].geometry.coordinates;
  }
};
</script>

<style></style>
