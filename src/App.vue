<template>
  <div id="app" class="container text-center mt-5">
    <ElementTable
      :elementCount="Number(9)"
      :dataToShow="{
        tableHeadings: ['Id', 'Name', 'Magnitude', 'Miss Distance', 'Remove'],
        dataPoints: [
          'name',
          'absolute_magnitude_h',
          'close_approach_data[0][miss_distance.miles]',
        ],

        // OPTIONAL:  put each object property into array until you get the data you want in table
        subDataPath: ['near_earth_objects', '2019-09-08'],
      }"
      heading="Near Earth Objects"
      url="https://api.nasa.gov/neo/rest/v1/feed?start_date=2019-09-07&end_date=2019-09-08&api_key=VkhjpIiZWLF0DVij5GbHSRPVQFvNLHLZiQj1w6Lr"
    ></ElementTable>
    <ElementTable
      :elementCount="Number(50)"
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
import ElementTable from "./components/ElementTable";

export default {
  name: "App",

  data() {
    return {
      asteroids: [],
    };
  },
  components: {
    ElementTable,
  },
};
</script>

<style></style>
