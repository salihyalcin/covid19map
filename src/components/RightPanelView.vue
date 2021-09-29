<template>
  <div class="right_panel_view">
    <h3>Total Cases - World</h3>

    <h4>Vaccination Status</h4>
<!--    <ul>
      <li v-bind:key="vaccination.id"
          v-for="vaccination in vaccinations">
        {{ vaccination.name }}  {{ vaccination.fullyVaccinated.quote }}
      </li>
    </ul>-->

    <b-table striped hover :items="vaccinations" :fields="fields"></b-table>

<!--    <div>
      <b-table striped hover :items="items"></b-table>
    </div>-->

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
      errored: false,
      fields : [
        {
          key: 'name',
          label :'Name',
          sortable: true
        },
        {
          key: 'fullyVaccinated.quote',
          label : 'Fully Vaccinated %',
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
          this.vaccinations = response.data.data
          console.log(this.vaccinations)
        })
        .catch(error => {
          console.log(error)
        })
    //  .finally(() => this.loading = false)
  }
}


</script>

<style scoped>

.right_panel_view {
  padding: 0;
  margin: 10px;
  right: 0;
  position: absolute;
  width:fit-content;
  background-color: white;
  height: 100%;
  border-radius: 10px;

}

::v-deep .sr-only{display:none !important}

</style>