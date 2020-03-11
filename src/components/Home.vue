<template>
  <l-map
    class="maps"
    :bounds="bounds"
    @click="addMarker"
    :options="{ zoomControl: false }"
  >
    <l-tile-layer :url="url"></l-tile-layer>
    <l-control-zoom position="bottomright"></l-control-zoom>
    <MyMarker v-for="marker of markers" :key="marker.id" :marker="marker" />
    <l-geo-json :geojson="geojson" @add="setGeojsonBounds"></l-geo-json>
  </l-map>
</template>

<script>
import { LMap, LTileLayer, LGeoJson, LControlZoom } from "vue2-leaflet";
import { geojsonData } from "../geojsonData";
import { mapGetters, mapActions } from "vuex";
import { ADD_COORDINATE } from "../store/mutationTypes";
import { polygon } from "point-in-geopolygon";
import MyMarker from "./MyMarker.vue";

export default {
  name: "Home",
  components: {
    LMap,
    LTileLayer,
    LGeoJson,
    LControlZoom,
    MyMarker
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
      const isInside = polygon(this.polygon, [lng, lat]);

      if (isInside) {
        this.ADD_COORDINATE({
          position: {
            latitude: lat,
            longitude: lng
          }
        });
      }
    },
    setGeojsonBounds(e) {
      this.bounds = e.target.getBounds();
    },
    ...mapActions([ADD_COORDINATE])
  },
  computed: {
    ...mapGetters(["markers"])
  },
  created() {
    this.geojson = geojsonData;
    this.polygon = this.geojson.features[0].geometry.coordinates;
  }
};
</script>

<style></style>
