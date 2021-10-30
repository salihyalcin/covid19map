<template>
  <div class="right_panel_view">
    <h5>Last Update : {{lastUpdate}}</h5>

    <h5>Vaccination Status</h5>

    <b-table striped hover :items="vaccinations" :fields="fields"></b-table>

  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "RightPanelView",
  data() {
    return {
      vaccinations: null,
      loading: true,
      lastUpdate : null,
      fields: [
        {
          key: 'name',
          label: 'Name',
          sortable: true
        },
        {
          key: 'fullyVaccinated.quote',
          label: 'Fully Vaccinated %',
          sortable: true
        },
        {
          key: 'vaccinatedAtLeastOnce.quote',
          label: 'Vaccinated Once %',
          sortable: true
        }
      ]
    }
  },
  mounted() {
    axios
        .get('https://rki-vaccination-data.vercel.app/api/v2')
        .then(response => {
          this.vaccinations = response.data.data.filter(vacc => vacc.isState)
          this.vaccinations.push(response.data.data.at(-1))
          this.lastUpdate = response.data.lastUpdate
        })
        .catch(error => {
          console.log(error)
        })
  },
}


</script>

<style scoped>

.right_panel_view {
  width: 22.5%;
  height: 100%;
  position: relative;
  float: right;
  border-collapse: collapse;
}

::v-deep .sr-only{display:none !important}

</style>