<template>
  <div class="left_panel_view">
    <h3>Total Cases - Germany</h3>
    <p>{{currentDate()}}</p>
    <section v-if="errored">
      <p>We're sorry, we're not able to retrieve this information at the moment, please try back later</p>
    </section>

    <section v-else>
      <b-table striped hover :items="cases" :fields="fields" ></b-table>

    </section>
  </div>
</template>

<script>

import axios from "axios";

export default {
  data () {
    return {
      cases : null,
      info: null,
      loading: true,
      errored: false,
      fields: [
        {
          key: 'Province_State',
          label: 'Name'
        },
        {
          key: 'Confirmed',
          label: 'Cases',
          sortable : true
        },
        {
          key: 'Deaths',
          label: 'Deaths',
          sortable : true
        }
      ]
    }
  },
  mounted() {
    axios
        .get('https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_daily_reports/'+this.requestDate()+'.csv')
        .then(response => {
          const input = response.data
          const lines = input.split('\n') // 1️⃣
          const header = lines[0].split(',') // 2️⃣
          const output = lines.slice(1).map(line => {
            const fields = line.split(',') // 3️⃣
            return Object.fromEntries(header.map((h, i) => [h, fields[i]])) // 4️⃣
          })
          this.cases = output.filter(vacc => vacc.Country_Region==="Germany" && vacc.Lat !=="")

          const formatCash = n => {
            if (n < 1e3) return n;
            if (n >= 1e3) return +(n / 1e3).toFixed(1) + "K";
          };

          var reformatCases = function(caseNumbers) {
            return caseNumbers.map(function(cas) {
              // create a new object to store full name.
              var newObj = {};
              newObj["Province_State"] = cas.Province_State;
              newObj["Confirmed"] = formatCash(parseInt(cas.Confirmed));
              newObj["Deaths"] = formatCash(parseInt(cas.Deaths));
              // return our new object.
              return newObj;
            });
          };

          this.cases = reformatCases(this.cases)
        })
        .catch(error => {
          console.log(error)
        })
  },
  name: "LeftPanelView",
  methods: {
    currentDate() {
      const current = new Date();
      return `${current.getDate()}/${current.getMonth() + 1}/${current.getFullYear()}`;
    },
    requestDate() {
      const current = new Date();
      return `${current.getMonth() + 1}-${current.getDate()-1}-${current.getFullYear()}`;
    }
  }
}

</script>

<style scoped>
.left_panel_view {
  width: 22.5%;
  height: 100%;
  float: left;
  border-collapse: collapse;
}

::v-deep .sr-only{display:none !important}
</style>