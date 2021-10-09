<template>
<div class="footer">
  <apexchart class="data-graph" v-if="getGermanyData" type="line" width= "1000" height="190" :options="chartOptions" :series="getTimeSeriesData"></apexchart>
</div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'FooterView',
  data () {
    return {
      data: null,
    }
  },
  computed: {
    getGermanyData() {
      if (this.data) {
        return this.data["Germany"]
      } else {
        return null
      }
    },
    getDates() {
      if (this.getGermanyData) {
        const dates = []
        this.getGermanyData.forEach(({date}) => dates.push(date))
        return dates
      } else {
        return null
      }
    },
    getTimeSeriesData() {
      if (this.getGermanyData) {
        const confirmedData = []
        // const recoveredData = []
        const deathsData = []
        // this.getGermanyData.forEach(({confirmed, recovered, deaths}) => {
        //   confirmedData.push(confirmed)
        //   recoveredData.push(recovered)
        //   deathsData.push(deaths)
        // })
        this.getGermanyData.forEach(({confirmed, deaths}) => {
          confirmedData.push(confirmed)
          deathsData.push(deaths)
        })
        return [
          {
            name: 'confirmed',
            data: confirmedData
          },
          // {
          //   name: 'recovered',
          //   data: recoveredData
          // },
          {
            name: 'deaths',
            data: deathsData
          },
        ]
      } else {
        return null
      }
    },
    chartOptions() {
      return {
        xaxis: {
          categories: this.getDates,
          tickAmount: 7
        }
      }
    },
  },
  mounted() {
    axios.get("https://pomber.github.io/covid19/timeseries.json")
      .then( ({ data }) => { this.data = data})
  }
}
</script>



<style>
body {
  overflow: hidden !important; /* Hide scrollbars */
}

.footer {
  width: 50%;
  height: 30%;
  border-collapse: collapse;
  float: left;
}
.data-graph {
  width: 50%;
  height: 30%;
  border-collapse: collapse;
  float: left;
}

</style>