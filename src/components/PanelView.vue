<template>
  <div class="panel_view"></div>
</template>

<script>

export default {
  name: "PanelView",
  created() {
    // Simple GET request using fetch
    fetch(
        "https://raw.githubusercontent.com/salihyalcin/map_assests/main/cases.json"
    )
        .then((response) => response.json())
        .then((data) => console.log(data));
  },
}


</script>

<style scoped>
.panel_view {
  padding: 0;
  margin: 0;
  position: absolute;
  top: 0;
  width: 20%;
  background-color: aquamarine;
  height: 100%;
}
</style>