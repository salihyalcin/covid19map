<template>
  <div id="app">
    <MapView />
    <LeftPanelView/>
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
    FooterView
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}


</style>
