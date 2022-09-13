<template>
  <div id="map" class="window-height"></div>
</template>

<script>
import tt from "@tomtom-international/web-sdk-maps";
const size = 50;
export default {
  name: "TomTomMap",
  props: ["city"],
  data() {
    return {
      currentLatitude: null,
      currentLongitude: null,
    };
  },
  watch: {
    currentLatitude: {
      handler(newVal) {
        this.initMap();
      },
    },
  },
  mounted() {
    this.getLocation();
    this.initMap();
  },
  methods: {
    initMap() {
      let map = tt.map({
        container: "map",
        key: "9ScPeM1DHHk5d3qeCGTO2RTC8LC73vu2",
        center: [this.currentLongitude, this.currentLatitude],
        zoom: 20,
      });
      map.on("load", () => {
        new tt.Marker()
          .setLngLat([this.currentLongitude, this.currentLatitude])
          .addTo(map);
      });
    },
    getLocation() {
      const successCallback = (position) => {
        console.log(position);
        let coords = position.coords;
        this.currentLatitude = coords.latitude;
        this.currentLongitude = coords.longitude;
        console.log(this.currentLatitude, this.currentLongitude);
      };

      const errorCallback = (error) => {
        console.log(error);
      };
      const options = {
        enableHighAccuracy: true,
      };
      navigator.geolocation.getCurrentPosition(
        successCallback,
        errorCallback,
        options
      );
    },
  },
};
</script>
