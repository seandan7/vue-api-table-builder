<template>
  <div id="app" class="container text-center mt-5">
    <Form></Form>
    <ElementTable
      :dataToShow="{
        tableHeadings: ['Id', 'Name', 'Magnitude', 'Miss Distance', 'Remove'],
        dataPoints: [
          'name',
          'absolute_magnitude_h',
          'close_approach_data[0].miss_distance.miles',
        ],
      }"
      heading="Near Earth Objects"
      url="https://api.nasa.gov/neo/rest/v1/feed?start_date=2019-09-07&end_date=2019-09-07&api_key=VkhjpIiZWLF0DVij5GbHSRPVQFvNLHLZiQj1w6Lr"
    ></ElementTable>
  </div>
</template>

<script>
import Form from "./components/Form.vue";
import ElementTable from "./components/ElementTable";

export default {
  name: "App",

  data: function() {
    return {
      asteroids: [],
    };
  },
  components: {
    Form,
    ElementTable,
  },
};
</script>

<style></style>
