<template>
  <div class="card mt-5">
    <h2 class="card-header">Near Earth Objects</h2>
    <div class="m-33" v-cloak v-if="elements.length > 0">
      <p>showing {{ elements.length }} items</p>
      <p>{{ closestObj }} has the shortest miss</p>
    </div>
    <table class="table table-striped">
      <thead class="thead-dark">
        <th>id</th>
        <th>Name</th>
        <th>Close Approach</th>
        <th>Miss Distance</th>
        <th>Remove</th>
      </thead>
      <tbody v-cloak>
        <tr
          v-for="(element, index) in elements"
          :key="element.id"
          :class="{
            highlight: isOdd(index),
            'font-weight-bold': !isOdd(index),
          }"
        >
          <td>{{ index + 1 }}</td>
          <td>{{ element.name }}</td>
          <td>{{ getCloseApproachDate(element) }}</td>
          <td>
            <ul
              v-if="element.close_approach_data.length > 0"
              class="list-unstyled text-left border-left pl-2 "
            >
              <li
                v-for="(value, key) in element.close_approach_data[0]
                  .miss_distance"
                :key="key"
              >
                <b>{{ key }}:</b> {{ value }}
              </li>
            </ul>
          </td>
          <td>
            <button @click.stop="remove(index)" class="btn btn-warning">
              remove
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "NasaTable",
  computed: {
    closestObj: function() {
      var hasData = this.elements.filter((item) => {
        return item.close_approach_data.length > 0;
      });
      console.log(hasData);
      var simpleNoes = hasData.map((item) => {
        return {
          name: item.name,
          miles: item.close_approach_data[0].miss_distance.miles,
        };
      });
      var sortedneos = simpleNoes.sort((a, b) => {
        return a.miles - b.miles;
      });
      return sortedneos[0].name;
    },
  },
  methods: {
    getCloseApproachDate: function(a) {
      if (a.close_approach_data.length > 0) {
        return a.close_approach_data[0].close_approach_date;
      } else {
        return "N/a";
      }
    },

    remove: function(index) {
      this.elements.splice(index, 1);
    },
    isOdd: function(idx) {
      return idx % 2 === 0;
    },
  },
  props: {
    elements: Array,
  },
};
</script>
<style scoped>
.highlight {
  color: blue;
}
</style>
