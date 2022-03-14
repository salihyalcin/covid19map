<template>
  <div id="map">
  </div>
</template>

<script>
import MapView from "@arcgis/core/views/MapView";
import Map from "@arcgis/core/Map";
import GeoJSONLayer from "@arcgis/core/layers/GeoJSONLayer";
import LabelClass from "@arcgis/core/layers/support/LabelClass";

export default {
  data() {
    return {
      germanyCases: null
    }
  },
  name: 'MapView',
  props: {
    cases: Array
  },
  async mounted() {
    const self = this
    //Creating basemap
    const map = new Map({
      basemap: "streets-vector"
    });
    this.germanyCases = this.cases
    const stateTemplate = {
      //  test : this.cases[this.cases.findIndex(el => el.Province_State==={name})].Confirmed,
      title: "{name} Covid Status",
      content: showPopupInformation
    };
    // this.counties[filtered].Confirmed
    /*   const countyTemplate = {
         title: "{NAME_2} Covid Status",
         content: "County Level",
       };*/

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

    //Creating mapView that holds our Map
    let mapView = new MapView({
      container: "map",
      map: map,
      zoom: 5,
      center: [10.3314223, 51.1469843]
    });

    //Initializing states layer as GeoJSONLayer
    const statesLayer = new GeoJSONLayer({
      url: "https://raw.githubusercontent.com/isellsoap/deutschlandGeoJSON/main/2_bundeslaender/4_niedrig.geo.json",
      displayField: "name",
      popupTemplate: stateTemplate
      //renderer: renderer
    });

    //Initializing counties layer as GeoJSONLayer
    const countiesLayer = new GeoJSONLayer({
      url: "https://raw.githubusercontent.com/salihyalcin/covid19map/master/map.geojson",
      displayField: "name"
      // popupTemplate: countyTemplate
    });

    statesLayer.labelingInfo = new LabelClass({
      labelExpressionInfo: {expression: "$feature.name"},
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
    //Zooming out zooming in - Arranging layers.
    mapView.watch("zoom", (newValue) => {
      if (newValue >= 7) {
        countiesLayer.visible = true
        countiesLayer.labelsVisible = true
        statesLayer.visible = false
        statesLayer.labelsVisible = false
      } else {
        countiesLayer.visible = false
        countiesLayer.visible = false
        statesLayer.visible = true
        statesLayer.labelsVisible = true
      }
    });


    map.add(countiesLayer)
    countiesLayer.labelsVisible = false
    map.add(statesLayer)

    for (let i = 0; i < this.cases.length; i++) {
      this.cases[i].Province_State = this.cases[i].Province_State.replace('ü', 'u');
    }


    //console.log(this.cases[this.cases.findIndex(el => el.Province_State==="Berlin")].Confirmed)

    function showPopupInformation(feature) {
      const div = document.createElement("div");

      const selectedState = feature.graphic.attributes.name.replace("ü", "u")

      div.innerHTML = "Total case number : "+ self.cases[self.cases.findIndex(el => (el.Province_State) === selectedState)].Confirmed;
      return div;


    }

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
@import 'https://js.arcgis.com/4.19/@arcgis/core/assets/esri/themes/light/main.css';

html,
body,
#map {
  width: 55%;
  height: 70%;
  float: left;
  border-collapse: collapse;
}

</style>
