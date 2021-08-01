<template>
  <div class="home">
    <h1>{{ message }}</h1>
     <!-- <button v-on:click="doMapbox()">Do Mapbox</button> -->
    <div id='map' style='width: 400px; height: 300px;'></div>
  </div>
</template>

<style>
#marker {
  background-image: url("https://docs.mapbox.com/mapbox-gl-js/assets/washington-monument.jpg");
  background-size: cover;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  cursor: pointer;
}

.mapboxgl-popup {
  max-width: 200px;
}
</style>

<script>
import mapboxgl from "mapbox-gl"; // or "const mapboxgl = require('mapbox-gl');"

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      places: [
        { lat: -25.363, lng: 131.044, description: "A place in Australia" },
        { lat: -33.8675, lng: 151.207, description: "The main city!" },
      ],
    };
  },
  mounted: function () {
    this.doMapbox();
  },
  methods: {
    doMapbox: function () {
      mapboxgl.accessToken = "";
      var monument = [-77.0353, 38.8895];
      // console.log(process.env.VUE_APP_MAPBOX_API_KEY)
      const map = new mapboxgl.Map({
        container: "map", // container ID
        style: "mapbox://styles/mapbox/satellite-streets-v11", // style URL
        center: monument,
        zoom: 5,
      });
      // create the popup
      var popup = new mapboxgl.Popup({ offset: 25 }).setText("Construction on the Washington Monument began in 1848.");

      // create DOM element for the marker
      var el = document.createElement("div");
      el.id = "marker";

      // create the marker
      new mapboxgl.Marker(el)
        .setLngLat(monument)
        .setPopup(popup) // sets a popup on this marker
        .addTo(map);

      // loop through places and make a new marker for each element
      this.places.forEach(function (place) {
        console.log(place);
        // create the popup
        var popup = new mapboxgl.Popup({ offset: 25 }).setText(place.description);
        new mapboxgl.Marker().setLngLat([place.lng, place.lat]).setPopup(popup).addTo(map);
      });

      // Create a default Marker, colored black, rotated 45 degrees.
      var marker2 = new mapboxgl.Marker({ color: "black", rotation: 45 }).setLngLat([12.65147, 55.608166]).addTo(map);
    },
  },
};
</script>