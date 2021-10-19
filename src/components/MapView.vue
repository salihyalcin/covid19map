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
import Query from "@arcgis/core/tasks/support/Query";
import * as queryChargingStationsTask from "@arcgis/core/core/workers/request";

export default {
  name: 'MapView',
  props: {
    cases: Array
  },
  async mounted() {
     const map = new Map({
      basemap: "streets-vector"
    });

    /*const stateTemplate = {
      title: "{name} Covid Status",
      content: "{this.cases[this.cases.findIndex(el => el.Province_State===\"Berlin\")].Confirmed}",
    };*/
   // this.counties[filtered].Confirmed
  /*  const countyTemplate = {
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

    function queryChargingStations(target) {
      // count and types of electric charging stations that intersect the county

      const query = new Query({
        geometry: target.graphic.geometry,
        outFields: ["*"],
        spatialRelationship: "intersects"
      });

      // execute the query task
      return queryChargingStationsTask.execute(query).then((result) => {
        const stats = result.features[0].attributes;
        console.log(stats)
        // format the query result for the counties popupTemplate's content.
        /*  return "<b>" + (stats.numLevel_1 + stats.numLevel_2 + stats.numLevel_3) +
              "</b> electric charging stations intersect the boundary of {NAME}. <br><br>" +
              "The number and type of stations: <br>" +
              "<ul>" +
              "<li> Level 1 Charging Stations (120V, AC): " + (stats.numLevel_1) + "</li>" +
              "<li> Level 2 Charging Stations (240V, AC): " + (stats.numLevel_2) + "</li>" +
              "<li> Level 3 Charging Stations (480V, DC): " + (stats.numLevel_3) + "</li>" +
              "</ul>";*/
      });
    }

    let mapView = new MapView({
      container: "map",
      map: map,
      zoom: 5,
      center: [10.3314223, 51.1469843]
    });

    const statesLayer = new GeoJSONLayer({
      url: "https://raw.githubusercontent.com/isellsoap/deutschlandGeoJSON/main/2_bundeslaender/4_niedrig.geo.json",
      displayField: "name",
      popupTemplate: queryChargingStations(statesLayer())
      //renderer: renderer
    });

    const countiesLayer = new GeoJSONLayer({
      url: "https://raw.githubusercontent.com/salihyalcin/map_assests/main/map.geojson",
      displayField: "name"
     // popupTemplate: countyTemplate
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

    mapView.watch("zoom", (newValue) => {
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


    console.log(this.cases[this.cases.findIndex(el => el.Province_State==="Berlin")].Confirmed)

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
