<template>
  <div id="app">
<!--
    <b-col class="right-panel"></b-col>
    <b-col class="map-view"></b-col>
    <b-col class="left-panel"></b-col>
    <b-col class="footer-view"></b-col>-->
    <LeftPanelView/>
    <MapView />
    <RightPanelView/>
    <FooterView/>
  </div>
</template>

<script>
import MapView from './components/MapView.vue'
import RightPanelView from "@/components/RightPanelView";
import LeftPanelView from "@/components/LeftPanelView";
import FooterView from "@/components/FooterView";
export default {
  name: 'App',
  components: {
    LeftPanelView,
    MapView,
    RightPanelView,
    FooterView,
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  width: 100%;
  height: 100%;

}
body, html {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 5px;
}

h6, .h6, h5, .h5, h4, .h4, h3, .h3, h2, .h2, h1, .h1 {
  font-weight: 500;
  line-height: 1.2;
  margin: 1.5rem;
}


</style>
