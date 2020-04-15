<template>
  <div class="card mt-5">
    <h2 class="card-header">
      {{ heading }}
    </h2>
    <div class="overflow-hidden">
      <transition name="height-shrink">
        <div class="m-33" v-cloak v-if="elements.length > 0 && showSummary">
          <p>showing {{ elements.length }} items</p>
          <p>{{ closestObj }} has the shortest miss</p>
        </div>
      </transition>
    </div>
    <a href="#" @click="showSummary = !showSummary">Toggle summary</a>
    <table class="table table-striped table-dark">
      <thead>
        <th
          :key="index"
          v-for="(element, index) in tableHeadings"
          class="thead-dark"
        >
          {{ element }}
        </th>
      </thead>
      <tbody is="transition-group" name="neo-list" tag="tbody" v-cloak>
        <tr
          v-for="(element, index) in elements"
          :key="element.id"
          class="overflow-hidden"
          :class="{
            highlight: isOdd(index),
            'font-weight-bold': !isOdd(index),
          }"
        >
          <td v-for="(dataPoint, index) in dataToShow" :key="index">
            {{ element + "." + dataPoint }}
          </td>
          <td>{{ element.name }}</td>
          <td>{{ element.absolute_magnitude_h }}</td>
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
  data() {
    return {
      showSummary: true,
      elements: [],
    };
  },
  computed: {
    closestObj: function() {
      var hasData = this.elements.filter((item) => {
        return item.close_approach_data.length > 0;
      });
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
  created() {
    fetch(this.url)
      .then((response) => {
        return response.json();
      })
      .then((data) => {
        this.elements = data.near_earth_objects["2019-09-07"].slice(0, 10);
      });
  },
  methods: {
    remove: function(index) {
      this.elements.splice(index, 1);
    },
    isOdd: function(idx) {
      return idx % 2 === 0;
    },
  },
  props: {
    url: String,
    heading: String,
    tableHeadings: Array,
    dataToShow: Array,
  },
};
</script>
<style scoped>
.highlight {
  color: lightblue;
}
.height-shrink-leave-to,
.height-shrink-enter {
  max-height: 0px !important;
}
.height-shrink-enter-active,
.height-shrink-leave-active {
  max-height: 500px;
  transition: all 0.2s;
}

.neo-list-leave-to,
.neo-list-enter {
  opacity: 0;
  transform: translateY(30px);
}
.neo-list-enter-active,
.neo-list-leave-active {
  transition: all 0.2s linear;
}

.spin-enter-active {
  animation: spin-steps 2s;
}
@keyframes spin-steps {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(2);
  }
  100% {
    transform: scale(1);
  }
}
</style>
