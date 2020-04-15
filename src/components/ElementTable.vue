<template>
  <div class="card mt-5">
    <h2 class="card-header">
      {{ heading }}
    </h2>
    <div class="overflow-hidden">
      <transition name="height-shrink">
        <div class="m-33" v-cloak v-if="elements.length > 0 && showSummary">
          <p>showing {{ elements.length }} items</p>
        </div>
      </transition>
    </div>
    <button
      class="btn btn-primary btn-sm d-inline-block"
      @click.stop="showSummary = !showSummary"
    >
      Toggle summary
    </button>
    <table class="table table-striped table-dark">
      <thead>
        <th
          :key="index"
          v-for="(element, index) in dataToShow.tableHeadings"
          class="thead-dark"
        >
          {{ element }}
        </th>
      </thead>
      <tbody :key="index" v-for="(element, index) in elements">
        <tr>
          <td>{{ index + 1 }}</td>
          <td v-for="(dataPoint, index) in dataToShow.dataPoints" :key="index">
            {{ element[dataPoint] }}
            {{ log(element[dataPoint]) }}
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
  name: "ElementTable",
  data() {
    return {
      showSummary: true, // total items toggler
      elements: [], // this populates on created,
    };
  },

  created() {
    fetch(this.url)
      .then((response) => {
        return response.json();
      })
      .then((data) => {
        // this accounts for the subdata in NASA's API two levels. TODO: Refactor
        if (this.dataToShow.subData) {
          if (this.dataToShow.subSubData) {
            this.elements =
              data[this.dataToShow.subData][this.dataToShow.subSubData];
          } else {
            this.elements = data[this.dataToShow.subData];
          }
        } else {
          this.elements = data;
        }
        // only show number of elements specified. TODO: Allow prop for if url has limit param
        this.elements.splice(this.elementCount);
      });
  },
  methods: {
    log(message) {
      console.log(message);
    },
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
    dataToShow: Object,
    elementCount: Number,
  },
};
</script>
<style scoped>
.height-shrink-leave-to,
.height-shrink-enter {
  max-height: 0px !important;
}
.height-shrink-enter-active,
.height-shrink-leave-active {
  max-height: 500px;
  transition: all 0.2s;
}
</style>
