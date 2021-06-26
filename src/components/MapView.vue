<template>
  <div id="map">
  </div>

</template>

<script>
import MapView from "@arcgis/core/views/MapView";
import Map from "@arcgis/core/Map";
import GeoJSONLayer from "@arcgis/core/layers/GeoJSONLayer";

export default {
  name: 'MapView',

  async mounted() {
    const map = new Map({
      basemap: "streets-vector"
    });

    const template = {
      title: "{NAME_2} Covid Status",
      content: "Asdk",
    };

    // eslint-disable-next-line no-unused-vars
    const renderer = {
      type: "simple",
      field: "NAME_2",
      symbol: {
        type: "simple-marker",
        color: "orange",
        outline: {
          color: "white"
        }
      },
      visualVariables: [{
        type: "size",
        field: "NAME_2",
        stops: [{
          value: 2.5,
          size: "4px"
        },
          {
            value: 8,
            size: "40px"
          }
        ]
      }]
    };

    // eslint-disable-next-line no-unused-vars
    let view = new MapView({
      container: "map",
      map: map,
      zoom: 6,
      center: [10.3314223, 51.1469843]
    });

    const geoJSONLayer = new GeoJSONLayer({
      url: "https://raw.githubusercontent.com/salihyalcin/map_assests/main/map.geojson",
      displayField:"NAME_2",
      popupTemplate: template
      //renderer: renderer
    });

    map.add(geoJSONLayer)


  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
@import 'https://js.arcgis.com/4.19/@arcgis/core/assets/esri/themes/light/main.css';

html,
body,
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding: 0;
  margin: 0;
  width: 100%;
  height: 100%;
}

.main_view {
  padding: 0;
  margin: 0;
  width: 100%;
  height: 100%;
}

#map {
  padding: 0;
  margin: 0;
  width: 100%;
  height: 100%;
  position: absolute;
  bottom: 0;
}
</style>
