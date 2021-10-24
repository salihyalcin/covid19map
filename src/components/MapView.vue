<template>
  <div id="map">
    <!--  {{ info }}-->
  </div>

</template>

<script>
import MapView from "@arcgis/core/views/MapView";
import Map from "@arcgis/core/Map";
import GeoJSONLayer from "@arcgis/core/layers/GeoJSONLayer";
import LabelClass from "@arcgis/core/layers/support/LabelClass";
/*import Query from "@arcgis/core/tasks/support/Query";
import * as queryChargingStationsTask from "@arcgis/core/core/workers/request";*/

export default {
  name: 'MapView',
  props: {
    cases: Array
  },
  async mounted() {
    const map = new Map({
      basemap: "streets-vector"
    });

    const stateTemplate = {
      //  test : this.cases[this.cases.findIndex(el => el.Province_State==={name})].Confirmed,
      title: "{name} Covid Status",
      content: populationChange,
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
    //deployment vercel
    //test vercel
    // eslint-disable-next-line no-unused-vars
    let mapView = new MapView({
      container: "map",
      map: map,
      zoom: 5,
      center: [10.3314223, 51.1469843]
    });

    const statesLayer = new GeoJSONLayer({
      url: "https://raw.githubusercontent.com/isellsoap/deutschlandGeoJSON/main/2_bundeslaender/4_niedrig.geo.json",
      displayField: "name",
      popupTemplate: stateTemplate
      //renderer: renderer
    });

    const countiesLayer = new GeoJSONLayer({
      url: "https://raw.githubusercontent.com/salihyalcin/map_assests/main/map.geojson",
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


    //console.log(this.cases[this.cases.findIndex(el => el.Province_State==="Berlin")].Confirmed)

    function populationChange(feature) {
      const div = document.createElement("div");
      //const upArrow = '<svg width="16" height="16" ><polygon points="14.14 7.07 7.07 0 0 7.07 4.07 7.07 4.07 16 10.07 16 10.07 7.07 14.14 7.07" style="fill:green"/></svg>';
      //  const downArrow = '<svg width="16" height="16"><polygon points="0 8.93 7.07 16 14.14 8.93 10.07 8.93 10.07 0 4.07 0 4.07 8.93 0 8.93" style="fill:red"/></svg>';

      // Calculate the population percent change from 2010 to 2013.
     // const diff = feature.graphic.attributes.POP2013 - feature.graphic.attributes.POP2010;
      //const arrow = diff > 0 ? upArrow : downArrow;

      // Add green arrow if the percent change is positive and a red arrow for negative percent change.
      div.innerHTML =
          "As of 2010, the total population in this area was <b>" + feature.name + "</b> and the density was <b>" + feature.graphic.attributes.POP10_SQMI + "</b> sq mi. As of 2013, the total population was <b>" + feature.graphic.attributes.POP2013 + "</b> and the density was <b>" + feature.graphic.attributes.POP13_SQMI + "</b> sq mi. <br/> <br/>" +
          "Percent change is " +
          feature.name +
          "<span style='color: ";
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
