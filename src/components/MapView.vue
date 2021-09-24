<template>
  <div id="map">
    {{ info }}
  </div>

</template>

<script>
import MapView from "@arcgis/core/views/MapView";
import Map from "@arcgis/core/Map";
import GeoJSONLayer from "@arcgis/core/layers/GeoJSONLayer";
import LabelClass from "@arcgis/core/layers/support/LabelClass";

export default {
  name: 'MapView',
  /*created() {
    // Simple GET request using fetch
    fetch(
        "https://raw.githubusercontent.com/salihyalcin/map_assests/main/cases.json"
    )
        .then((response) => response.json())
        .then((data) => console.log(data));
  },*/

  async mounted() {
     const map = new Map({
      basemap: "streets-vector"
    });

    const stateTemplate = {
      title: "{name} Covid Status",
      content: "Test",
    };

    const countyTemplate = {
      title: "{NAME_2} Covid Status",
      content: "Test",
    };


    // eslint-disable-next-line no-unused-vars
    const renderer = {
      type: "simple",
      field: "name",
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
      zoom: 5,
      center: [10.3314223, 51.1469843]
    });

    const statesLayer = new GeoJSONLayer({
      url: "https://raw.githubusercontent.com/isellsoap/deutschlandGeoJSON/main/2_bundeslaender/4_niedrig.geo.json",
      displayField: "name",
      popupTemplate: stateTemplate,
      label : "sadasf"
      //renderer: renderer
    });

    const countiesLayer = new GeoJSONLayer({
      url: "https://raw.githubusercontent.com/salihyalcin/map_assests/main/map.geojson",
      displayField: "name",
      popupTemplate: countyTemplate
    });

    statesLayer.labelingInfo = new LabelClass({
      labelExpressionInfo: { expression: "$feature.name" },
      symbol: {
        type: "text",  // autocasts as new TextSymbol()
        color: "black",
        haloSize: 1,
        haloColor: "white"
      }
    });


    countiesLayer.labelingInfo = new LabelClass({
      labelExpressionInfo: {expression: "$feature.NAME_2"},
      symbol: {
        type: "text",  // autocasts as new TextSymbol()
        color: "black",
        haloSize: 1,
        haloColor: "white"
      }
    })

    view.watch("zoom", (newValue) => {
      if(newValue >= 7){
        countiesLayer.visible = true
        countiesLayer.labelsVisible = true
        statesLayer.visible = false
        statesLayer.labelsVisible = false
      }
      else{
        countiesLayer.visible = false
        countiesLayer.visible = false
        statesLayer.visible = true
        statesLayer.labelsVisible = true
      }
    });

    map.add(countiesLayer)
    countiesLayer.labelsVisible = false
    map.add(statesLayer)



  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
@import 'https://js.arcgis.com/4.19/@arcgis/core/assets/esri/themes/light/main.css';

html,
body,

#map {
  padding: 0;
  margin: 0;
  width: 100%;
  height: 100%;
  position: absolute;
  bottom: 0;
}
</style>
