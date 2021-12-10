<template>
  <div class="footer">
    <apexchart class="data-graph" v-if="getGermanyData" type="line" width= "100%" height="190" :options="chartOptions" :series="getTimeSeriesData"></apexchart>
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
        //return this.data["Germany"]
        return this.data
      } else {
        return null
      }
    },
    getDates() {
      if (this.getGermanyData) {
        const dates = []
        this.getGermanyData.forEach(({Date}) => dates.push(Date))
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
        this.getGermanyData.forEach(({Confirmed, Deaths}) => {
          confirmedData.push(Confirmed)
          deathsData.push(Deaths)
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
          //tickAmount: 15,
          type: 'datetime'
        }
      }
    },
  },
  mounted() {
    // axios.get("https://pomber.github.io/covid19/timeseries.json")
    //   .then( ({ data }) => { this.data = data})

    axios.get("https://api.covid19api.com/country/germany?from=2019-03-01T00:00:00Z&to=2022-04-01T00:00:00Z")
        .then( ({ data }) => { this.data = data})
  }
}
</script>



<style>
body {
  overflow: hidden !important; /* Hide scrollbars */
}

.footer {
  width: 55%;
  height: 30%;
  border-collapse: collapse;
  float: left;
}
.data-graph {
  width: 100%;
  height: 30%;
  border-collapse: collapse;
  float: left;
}

</style>