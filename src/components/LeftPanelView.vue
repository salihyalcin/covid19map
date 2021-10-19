<template>
  <div class="left_panel_view">
    <h3>Total Cases - Germany</h3>
    <p>{{ currentDate() }}</p>
    <section v-if="errored">
      <p>We're sorry, we're not able to retrieve this information at the moment, please try back later</p>
    </section>

    <section v-else>
      <b-table striped hover :items="cases" :fields="fields"></b-table>

    </section>
  </div>
</template>

<script>

export default {
  data() {
    return {
      errored: false,
      fields: [
        {
          key: 'Province_State',
          label: 'Name'
        },
        {
          key: 'Confirmed',
          label: 'Cases',
          sortable: true
        },
        {
          key: 'Deaths',
          label: 'Deaths',
          sortable: true
        }
      ]
    }
  },
  props: {
      cases : Array
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
  width: 22.5%;
  height: 100%;
  float: left;
  border-collapse: collapse;
}

::v-deep .sr-only {
  display: none !important
}
</style>