<template>
  <div id="app">
    <MapView />
    <PanelView />
  </div>
</template>

<script>
import MapView from './components/MapView.vue'
import PanelView from "@/components/PanelView";

export default {
  name: 'App',
  components: {
    PanelView,
    MapView
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
