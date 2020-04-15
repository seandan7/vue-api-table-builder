<template>
  <div id="app" class="container text-center mt-5">
    <Form></Form>
    <NasaTable :elements="asteroids"></NasaTable>
  </div>
</template>

<script>
import Form from "./components/Form.vue";
import NasaTable from "./components/NasaTable";
var apiKey = "VkhjpIiZWLF0DVij5GbHSRPVQFvNLHLZiQj1w6Lr";
var url =
  "https://api.nasa.gov/neo/rest/v1/feed?start_date=2019-09-07&end_date=2019-09-07&api_key=" +
  apiKey;

export default {
  name: "App",
  created() {
    fetch(url)
      .then((response) => {
        return response.json();
      })
      .then((data) => {
        console.log(data);
        this.asteroids = data.near_earth_objects["2019-09-07"].slice(0, 10);
      });
  },
  data: function() {
    return {
      asteroids: [],
    };
  },
  components: {
    Form,
    NasaTable,
  },
};
</script>

<style></style>
