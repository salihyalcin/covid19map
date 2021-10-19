<template>
  <div id="app">
    <LeftPanelView v-bind:cases="totalCases"/>
    <MapView v-bind:cases="totalCases"/>
    <RightPanelView/>
    <FooterView/>
  </div>
</template>

<script>
import MapView from './components/MapView.vue'
import RightPanelView from "@/components/RightPanelView";
import LeftPanelView from "@/components/LeftPanelView";
import FooterView from "@/components/FooterView";
import axios from "axios";

export default {
  name: 'App',
  components: {
    LeftPanelView,
    MapView,
    RightPanelView,
    FooterView,
  },
  data() {
    return {
      totalCases: null
    }
  },
  mounted() {
    axios
        .get('https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_daily_reports/' + this.requestDate() + '.csv')
        .then(response => {
          const input = response.data
          const lines = input.split('\n') // 1️⃣
          const header = lines[0].split(',') // 2️⃣
          const output = lines.slice(1).map(line => {
            const fields = line.split(',') // 3️⃣
            return Object.fromEntries(header.map((h, i) => [h, fields[i]])) // 4️⃣
          })
          this.totalCases = output.filter(vacc => vacc.Country_Region === "Germany" && vacc.Lat !== "")
          const formatCash = n => {
            if (n < 1e3) return n;
            if (n >= 1e3) return +(n / 1e3).toFixed(1) + "K";
          };

          var reformatCases = function (caseNumbers) {
            return caseNumbers.map(function (cas) {
              // create a new object to store full name.
              var newObj = {};
              newObj["Province_State"] = cas.Province_State;
              newObj["Confirmed"] = formatCash(parseInt(cas.Confirmed));
              newObj["Deaths"] = formatCash(parseInt(cas.Deaths));
              // return our new object.
              return newObj;
            });
          };
          this.totalCases = reformatCases(this.totalCases)
        })
        .catch(error => {
          console.log(error)
        })
  },
  methods: {
    requestDate() {
      const current = new Date();
      return `${current.getMonth() + 1}-${current.getDate() - 1}-${current.getFullYear()}`;
    }
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
