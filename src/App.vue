<template>
  <div id="app" class="container text-center mt-5">
    <Form></Form>
    <ElementTable
      :elementCount="Number(9)"
      :dataToShow="{
        tableHeadings: ['Id', 'Name', 'Magnitude', 'Miss Distance', 'Remove'],
        dataPoints: [
          'name',
          'absolute_magnitude_h',
          'close_approach_data[0][miss_distance.miles]',
        ],
        subData: `near_earth_objects`,
        subSubData: '2019-09-07',
      }"
      heading="Near Earth Objects"
      url="https://api.nasa.gov/neo/rest/v1/feed?start_date=2019-09-07&end_date=2019-09-07&api_key=VkhjpIiZWLF0DVij5GbHSRPVQFvNLHLZiQj1w6Lr"
    ></ElementTable>
    <ElementTable
      elementCount="50"
      :dataToShow="{
        tableHeadings: ['Id', 'userId', 'title', 'completed', 'Remove'],
        dataPoints: ['userId', 'title', 'completed'],
      }"
      heading="User Test API"
      url="https://jsonplaceholder.typicode.com/todos/"
    >
    </ElementTable>
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
