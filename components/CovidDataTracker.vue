<template>
  <v-row>
    <template v-if="loading">
      <v-col cols="12">
        <v-skeleton-loader v-bind="attrs" class="mx-auto my-auto pa-5 pa-md-10" max-width="800px" style="height: 100vh; overflow: auto; overflow-y: hidden;" type="card"></v-skeleton-loader>
      </v-col>
    </template>
    <template v-else>
      <v-col class="text-center" cols="12">
        <h1 v-if="data.Country">{{ data.Country }}</h1>
        <h1 v-else>Global</h1>
        <h5>{{ new Date(data.Date) }}</h5>
        <h3>Last Update: {{ data.Date | diffForHumans }}</h3>
      </v-col>
      <Searchbar />
      <v-col cols="12" sm="12" md="6" lg="6" xl="6">
        <v-card color="light-blue darken-4" class="pa-5 text-center">
          <v-card-title class="justify-center"> TOTAL </v-card-title>
          <v-card-text class="d-flex justify-center align-center">
            <v-row>
              <v-col cols="12">Total Cases: {{ data.TotalConfirmed }}</v-col>
              <v-col cols="12">Total Deaths: {{ data.TotalDeaths }}</v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" sm="12" md="6" lg="6" xl="6">
        <v-card color="light-blue darken-4" class="pa-5 text-center">
          <v-card-title class="justify-center"> NEW </v-card-title>
          <v-card-text class="d-flex justify-center align-center">
            <v-row>
              <v-col cols="12">New Cases: {{ data.NewConfirmed }}</v-col>
              <v-col cols="12">New Deaths: {{ data.NewDeaths }}</v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </template>
  </v-row>
</template>

<script>
  import dayjs from "dayjs";
  import relativeTime from "dayjs/plugin/relativeTime";
  export default {
    filters: {
      diffForHumans: (date) => {
        if (!date) {
          return null;
        }
        return dayjs(date).fromNow();
      },
    },
    data() {
      return {
        attrs: {
          class: "mb-6",
          boilerplate: true,
          elevation: 2,
        },
        loading: true,
      };
    },
    watch: {
      data() {
        this.loading = false;
      },
    },
    computed: {
      data() {
        return this.$store.getters.data;
      },
      countries() {
        return this.$store.getters.countries;
      },
    },
    created() {
      dayjs.extend(relativeTime);
      this.$store.dispatch("FETCH_SUMARRRY");
    },
  };
</script>

<style></style>
