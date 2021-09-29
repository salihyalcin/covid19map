<template>
  <div class="left_panel_view">
    <h3>Total Cases - Germany</h3>
    <p>{{currentDate()}}</p>
    <section v-if="errored">
      <p>We're sorry, we're not able to retrieve this information at the moment, please try back later</p>
    </section>

    <section v-else>
      <div v-if="loading">Loading...</div>

      <div
          v-bind:key="cases.name"
          v-for="cases in info"
          class="case"
      >
        {{ cases.Province_State }}:
        <span class="lighten">
      <span v-html="cases.Active"></span>
    </span>
      </div>

    </section>
  </div>
</template>

<script>

import axios from "axios";

export default {
  data () {
    return {
      info: null,
      loading: true,
      errored: false
    }
  },
  mounted() {
    axios
        .get('https://raw.githubusercontent.com/salihyalcin/map_assests/main/cases.json')
        .then(response => {
          this.info = response.data
        })
        .catch(error => {
          console.log(error)
          this.errored = true
        })
        .finally(() => this.loading = false)
  },
  name: "LeftPanelView",
  methods: {
    currentDate() {
      const current = new Date();
      return `${current.getDate()}/${current.getMonth() + 1}/${current.getFullYear()}`;
    }
  }
}


</script>

<style scoped>
.left_panel_view {
  padding: 0;
  margin: 10px;
  position: absolute;
  top: 0;
  width: fit-content;
  background-color: white;
  height: 100%;
  border-radius: 10px;
}
</style>