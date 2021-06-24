<template>
  <div id="map">
  </div>

</template>

<script>
import MapView from "@arcgis/core/views/MapView";
import Map from "@arcgis/core/Map";

//import Extent from "@arcgis/core/geometry/Extent";

/*/*
import Bookmarks from "@arcgis/core/widgets/Bookmarks";
import Expand from "@arcgis/core/widgets/Expand";*/

export default {
  name: 'MapView',
  async mounted() {
    const map = new Map({
      basemap: "streets-vector"
    });
    /*
        const webmap = new WebMap({
          portalItem: {
            id: "aa1d3f80270146208328cf66d022e09c",
          },
        });*/
    // eslint-disable-next-line no-unused-vars
    let view = new MapView({
      container: "map",
      map: map,
      zoom: 6,
      center: [10.3314223, 51.1469843]
    });

 /*   view.extent = new Extent({
      xmin: -9177882,
      ymin: 4246761,
      xmax: -9176720,
      ymax: 4247967,
      spatialReference: {
        wkid: 102100
      }
    });*/
    /*  const bookmarks = new Bookmarks({
        view: view,
        // allows bookmarks to be added, edited, or deleted
        editingEnabled: true,
      });
      const bkExpand = new Expand({
        view: view,
        content: bookmarks,
        expanded: true,
      });
      // Add the widget to the top-right corner of the view
      view.ui.add(bkExpand, "top-right");
      // bonus - how many bookmarks in the webmap?
      webmap.when(function () {
        if (webmap.bookmarks && webmap.bookmarks.length) {
          console.log("Bookmarks: ", webmap.bookmarks.length);
        } else {
          console.log("No bookmarks in this webmap.");
        }
      });*/
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
